---
published: true
layout: post
title:  "Avoiding Null In CSharp"
date:   2017-09-04 11:25:25 +0200
categories: csharp
---

In object oriented software we often struggle with null references, and miss-use them to pass some information to the client, like
that method was not executed successfully, or some item was not found.
This could lead to lot of bugs, unpredictable behaviour and less readable code.
Here are some ways me could use to reduce number of null references in our code.

## TrySomething methods

This is a common way of providing information on whether method executed successfully and avoiding return on null object if method fails. Chances are you encountered these when using C# primitive types and their TryParse methods, which operate on the same way.
    
```csharp
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
    
This is sort of "easy way out" and code is more predictable and readable. Downside is that this doesn't help as much and client code will still need to check returned boolean value. 
