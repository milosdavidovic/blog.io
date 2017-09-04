---
layout: post
title: Avoiding Null In CSharp
date: '2017-09-04 11:25:25 +0200'
categories: csharp
published: true
---
---
layout: post
title:  "Avoiding Null In CSharp"
date:   2017-09-04 11:25:25 +0200
categories: csharp
---

Some Ideas on how to eliminate null references in code

In object oriented software we often struggle with null references, and miss-use them to pass some information to the client, like
that method was not executed successfully, or some item was not found.
This could lead to lot of bugs, unpredictable behaviour and less readable code.
Here are some ways me could use to reduce number of null references in our code.

## TryXXX methods


This is a common way of providing information on whether method executed successfully and avoiding return on null object if method fails. Chances are you encountered these when using
C# primitive types and their TryParse methods, which operate on the same way.

	
```
public bool TrySendRequest(out string response)
{
	var client = new SomeClient();
	try
	{
		resonse = client.SendRequest();
		return true;
	}
	catch(Exception ex)
	{
		//log error
		return false;
	}
}
```

This is sort of "easy way out" and code is more predictable and readable. 
Downside is that this doesn't help as much and client code will still need to check returned boolean value. 


## Contitional result object 

This class is a kind a wrapper around actual object of interest, and it additionally provides Boolean property (Success) which indicates presence of the Result object.

	
```
class Conditional<T>
{
	public T Result { get; private set; }
	public bool Success { get; private set; }
	
	public Conditional()
	{
		this.Result = default(T);
		this.Success = false;
	}
	
	public Conditional(T value)
	{
		this.Result = value;
		this.Success = true;
	}
}
```

//	This approach was similar effect like TrySomething method and branching around Boolean value (Success) is still there.

## Option (Maybe) objects

Transform object to collection of objects of the same type implementing IEnumerable<T> interface and use the power of Linq to Objects to perform logic execution.
There are two constructors at our disposal. First accepts no parameters and it just creates empty collection of objects.
Other one is used in case we indeed have an object and it creates collection with only one item.
This Option class is now used to represent our object in form of a collection and it will never be null.
If collection is empty, no action will be executed and without exception. 


The problem
We are invoking method and expect some value to be returned. 
Problem here is that we are not sure if returned value will be null. It is certanlly possible.
So we have to protect our code with gourd statments, or NullReferenceException will arise. 
This leads to lot of if statments, but more importantlly,
every now and then someone will forget to check wether object is null and application could crash.
Here is one simple example of that situation:


```
static void Main(string[] args)
{
	Leprechaun lucky = new Leprechaun(new Gold(10));

	var stolenGold = lucky.SurrenderGold();

	if(stolenGold != null)
	{
		Console.WriteLine("{0} gold coins stolen from leprechaun.", gold.Amount);
	}
}

public class Leprechaun
{
	public Gold Stash { get; private set; }

	public Leprechaun(){ }
	
	public Leprechaun(Gold gold)
	{
		Gold = gold;
	}
	
	public Gold SurrenderGold()
	{
		return Stash;
	}
}

public class Gold
{
	public int Amount { get; set; } 
	
	public Gold(int amount)
	{
		Amount = amount;
	}
}
```

SurrenderGold method returns Gold object and we are not sure is it will contain some value or it will be null. So we need to check that in client code, in order to avoid raising exception.

This is where Option (or Maybe) objects come into place. We could change code so SurrenderGold method returns Option<T> (T is Gold in this case), 
and here is the new implementation:


```
public class Option<T> : IEnumerable<T>
{
	private readonly IEnumerable<T> values;

	private Option()
	{
		this.values = new T[0];
	}

	private Option(T value)
	{
		this.values = new T[1] { value };
	}

	public static Option<T> None() => new Option<T>();

	public static Option<T> Some(T value) => new Option<T>(value);

	public IEnumerator<T> GetEnumerator()
	{
		return this.values.GetEnumerator();
	}

	System.Collections.IEnumerator System.Collections.IEnumerable.GetEnumerator()
	{
		return this.values.GetEnumerator();
	}
}
```

Basicly, the idea is to wrap reference object into type extending IEnumerable<T> interface, 
and hence the object becomes a collection with zero or one object.
Now we have power of Linq To Objects at our disposal and client code can be written as follows:


```
static void Main(string[] args)
{
	Leprechaun lucky = new Leprechaun(new Gold(10));

	var stolenGold = lucky.SurrenderGold();

	foreach(var gold in stolenGold)
	{
		Console.WriteLine("{0} gold coins stolen from leprechaun.", gold);
	}
}

public class Leprechaun
{
	public Gold Stash { get; private set; }

	public Leprechaun(){ }
	
	public Leprechaun(Gold gold)
	{
		Stash = gold;
	}

	public Option<Gold> SurenderGold()
	{
		if (Stash != null)
			return Option<Gold>.Some(new Gold(10));
		else
			return Option<Gold>.None();
	}
}
```

Now we don't care if there is gold or not, becuse if collection is empty, nothig will be executed. 
We could also hide forech loop be creating extension for Enumerable class as follows:


```
public static class EnumerableExtensions
{
	public static void Do<T>(this Enumerable<T> sequence, Action<T> action)
	{
		foreach(T obj in sequence)
			action(obj);
	}
}
```

So final form of the client code should be little more readable and simpler:

```
static void Main(string[] args)
{
	Leprechaun lucky = new Leprechaun(new Gold(10));

	var stolenGold = lucky.SurrenderGold();

	stolenGold.Do(a=>
	{
		Console.WriteLine("{0} gold coins stolen from leprechaun.", a);
	});
}
```

This approach require some additional coding but as a reward allows us to write code in using Linq syntax 
and doesn't require null checks.
Probability of null realted bugs is reduced.

## Null Objects

Creating "null" class object eg. objects which are neutral respecting bussines logic. 
They are implementing same interface like real objects, but its actions don't cause any concrete functionality (non-functional objects).
This implementation shields the client from having to perform null checks.
Example of problematic situation:

	
```
public class Article
{
	public string Name {get; private set;}
	public decimal Price {get; private set;}
	private IDiscount _discount;
	
	public Article(string name, string price, IDiscount discount)
	{
		if(name == null)
			return new ArgumentException("name");
		if(price == null)
			return new ArgumentException("price");
		
		Name = name;
		Price = price;
		_discount = discount;
	}
	
	public decimal DiscountedPrice()
	{
		// Possiblity of _discount to be null forces us to perform check and branch
		if(_discount != null)
			return _discount.Apply(this.Price);
		else
			return this.Price;
	}
}

public Interaface IDiscount
{
	decimal Apply(decimal price)
}


public class NewCustomerDiscount : IDiscount
{
	public decimal Apply(decimal price)
	{
		return price * 0.95;
	}
}

public class HappyHourDiscount : IDiscount
{
	public decimal Apply(decimal price)
	{
		return decimal * 0.8;
	}
}
```

In this current implementation we expect IDiscount object to be passed to Article class through constructor
and problem arises when we don't want to give discount on the spesific Article. Only option we have now is
to pass null and that will signal that no discount should be applied. We use null to pass information.
This is bad as our intentions are not clear and one who wrote this code only knows this little sicret.
This effect maintanability and readability of the code.
Here is the example of the client code:


```
public static void Main (string[] args)
{
	var shoes = new Article("black shoes", 100.00, new HappyHourDiscount());
	var hat = new Article("red hat", 30.00, new NewCustomerDiscount());
	var jacket = new Article("blue jacket", 80.00, null); // We don't want to give discount on jackets
}
```
Passing null instead of discount object will cause ArgumentNull exception when calling Apply, so now code will branch on the null check to avoid this. One solution to this problem is to introduce the null object => Class that will implement the same interface like real objects, but it will behave like null eg. it will do nothing. In our example it will just return regular price with no discount, same befaviour like we had with null, but code looks a lot cleaner.

```
public class NoDiscount : IDiscount
{
	public decimal Apply(decimal price)
	{
		return price;
	}
}

public class Article
{
	public string Name {get; private set;}
	public decimal Price {get; private set;}
	private IDiscount _discount;
	
	public Article(string name, string price, IDiscount discount)
	{
		if(name == null)
			return new ArgumentException("name");
		if(price == null)
			return new ArgumentException("price");
		if(discount == null)
			return new ArgumentException("discount");
		Name = name;
		Price = price;
		_discount = discount;
	}
	
	public decimal DiscountedPrice()
	{
		return _discount.Apply(this.Price);
	}
}

public static void Main (string[] args)
{
	var shoes = new Article("black shoes", 100.00, new HappyHourDiscount());
	var hat = new Article("red hat", 30.00, new NewCustomerDiscount());
	var jacket = new Article("blue jacket", 80.00, new NoDiscount()); // Now we pass null object insted of just null
}
```

Null Object pattern makes code cleaner and more readable. Null object classes are often implemented as singletons, so only one instance is used troughout entire application.If Article constructor receives null as discount, Exeption will be raised at object creation level as it should,so we have no nasty surprizes in the later stages of code execution.


