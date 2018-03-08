---
layout: post
title:  "Angular Forms - The Template Approach"
date:   2018-03-01 16:27:25 +0100
categories: frontend
published: true
---

It is absolutely possible to create form and handle user input in the HTML, why would we need Angular's help to do this? Problem lies in the fact that we usually end up with submitting form's data to the server using the submit button. Due to the fact that we are creating SPA with help of the Angular framework and therefore there no "submitting", we need a different approach. Instead of the submitting, we can use Angular's Http Module to send form's data back and forth between our application and some backend server. Additionally, Angular helps us with this task by providing lot of functionality one might need when dealing with forms. Some of this functionality we get out-of-box with Angular are: easy access to form's data, user input validation, conditionally changes how form is displayed and what is visible or enabled and much more. 
There are two approaches to developing forms in Angular: Template and Reactive. This post will cover some of the basics of the Template approach through an example of creating simple pizza ordering form ('cuz everyone likes pizza!).

## Content

[Creating Simple Form](#creating-simple-form)

[Accessing Forms Data From Typescript](#accessing-forms-data-from-typescript)

[Using Select Element](#using-select-element)

[Using Radio Buttons](#using-radio-buttons)

[Using Checkbox Element](#using-checkbox-element)

[Adding Forms Validation](#adding-forms-validation)

[Grouping Forms Data](#grouping-forms-data)

[Default Values And Data Binding](#default-values-and-data-binding)

[Setting And Patching The Data](#setting-and-patching-the-data)

### Creating Simple Form

Lets start with a simple form, which has one input element and a submit button:  
![pizza-form-basic.png]({{ "/assets/2018-03-01-angular-forms/pizza-gui-basic.png" | relative_url }})

Let's create some basic HTML for demonstration purposes:

```html
<div>
  <form>
    <div class="container">
      <div class="row justify-content-md-center">
        <div class="col-md-6 ">
          <h1>Welcome to our pizza place!</h1>
          <hr />
        </div>
      </div>
      <div class="row">
        <div class="col-md-6">
          <h3>Create your pizza!</h3>
          <hr />
          <div class="form-group">
            <label for="name">Pizza Name</label>
            <input type="text" class="form-control" id="name">
          </div>
          <button class="btn btn-primary" type="submit" >Order Now</button>
        </div>
      </div>
    </div>
  </form>
</div>
```

### Accessing Forms Data From Typescript

Now, lets modify this form using the Angular syntax: 

```html
<div>
  <form (ngSubmit)="onSubmit(f)" #f="ngForm">
    <div class="container">
      <div class="row justify-content-md-center">
        <div class="col-md-6 ">
          <h1>Welcome to our pizza place!</h1>
          <hr />
        </div>
      </div>
      <div class="row">
        <div class="col-md-6">
          <h3>Create your pizza!</h3>
          <hr />
          <div class="form-group">
            <label for="name">Pizza Name</label>
            <input type="text" class="form-control" id="name" name="name" ngModel>
          </div>
          <button class="btn btn-primary" type="submit" >Order Now</button>
        </div>
      </div>
    </div>
  </form>
</div>
```

Here is the appropriate typescript to go with out form:

```ts
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  onSubmit(form: NgForm) {
    console.log('Value of a name variable is: ' + form.value.name);
  }
}

```

When we submit this form, we can see following output in the console as expected:

![pizza-console-basic.png]({{ "/assets/2018-03-01-angular-forms/pizza-console-basic.png" | relative_url }})

But to understand how this works, let examine what Angular exactly did for us here. We have our HTML form element, but it has some additional Angular syntax:  
`<form (ngSubmit)="onSubmit(f)" #pizzaForm="ngForm">`  
The `ngSubmit` is a directive which specifies the function to be executed when the form is submitted. In the example we have specified that onSubmit function should be executed and this function is defined in our typescript.  
What is interesting is that we were able to pass parameter f to this function, which is of type NgForm. NgForm is a another directive, and we can export a directive into a local template variable like: `#pizzaForm="ngForm"`.  
But this is just one peace of the puzzle.

`<input type="text" class="form-control" id="name" name="name" ngModel>`  
To finally get access to form values, we need to use another directive: `ngModel`, to tell Angular in what elements of the form are we interested in. also, we need to give name to our element using the name attribute, and this name we will also use when accessing the elements value in our typescript code.  
Finally we are able to get value from the input element as follows:  
`console.log('Value of a name variable is: ' + form.value.name);`

> HINT: We didn't have to pass `ngForm` object to the onSubmit function. Another approach would be to use Angular's ViewChild decorator and store our form data in the variable we define in our typescript class like:  
```ts
import { Component, ViewChild } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  @VeiwChild('pizzaForm') pizzaForm: NgForm;

  onSubmit() {
    console.log('Value of a name variable is: ' + pizzaForm.value.name);
  }
}
```
Don't forget to include ViewChild form `@angular/core`!

### Using Select Element

Lets add select element to our form to be able to choose pizza size:
![pizza-gui-select.png]({{ "/assets/2018-03-01-angular-forms/pizza-gui-select.png" | relative_url }})

Here is the HTML for the above example:
```html
<div>
  <form (ngSubmit)="onSubmit(f)" #f="ngForm">
    <div class="container">
      <div class="row justify-content-md-center">
        <div class="col-md-6 ">
          <h1>Welcome to our pizza place!</h1>
          <hr />
        </div>
      </div>
      <div class="row">
        <div class="col-md-6">
          <h3>Create your own pizza!</h3>
          <hr />
          <div class="form-group">
            <label for="name">Pizza Name</label>
            <input type="text" class="form-control" id="name" name="name" ngModel>
          </div>
          <div class="form-group">
            <label for="pizza-size">Choose Size</label>
            <select id="pizza-size" class="form-control" name="pizzaSize" ngModel>
              <option value="small">Small</option>
              <option value="medium">Medium</option>
              <option value="large">Large</option>
            </select>
          </div>
          <button class="btn btn-primary" type="submit">Order Now</button>
        </div>
      </div>
    </div>
  </form>
</div>
```
And the appropriate typescript code:
```ts
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {

  onSubmit(form: NgForm) {
    console.log('Chosen pizza size option is: ' + form.value.pizzaSize);
  }

}
```
We have manually selected _medium_ option for the pizza size, and successfully accessed this value using the same techniques as with the input element and the console shows the value:

![pizza-console-select.png]({{ "/assets/2018-03-01-angular-forms/pizza-console-select.png" | relative_url }})

### Using Radio Buttons

Lets add some radio buttons to be able to choose desired ketchup type:
![pizza-gui-radio.png]({{ "/assets/2018-03-01-angular-forms/pizza-gui-radio.png" | relative_url }})

And after adding code for this to out HTML, it looks like this:
```html
<div>
  <form (ngSubmit)="onSubmit(f)" #f="ngForm">
    <div class="container">
      <div class="row justify-content-md-center">
        <div class="col-md-6 ">
          <h1>Welcome to our pizza place!</h1>
          <hr />
        </div>
      </div>
      <div class="row">
        <div class="col-md-6">
          <h3>Create your own pizza!</h3>
          <hr />
          <div class="form-group">
            <label for="name">Pizza Name</label>
            <input type="text" class="form-control" id="name" name="name" ngModel>
          </div>
          <div class="form-group">
            <label for="pizza-size">Choose Size</label>
            <select id="pizza-size" class="form-control" name="pizzaSize" ngModel>
              <option value="small">Small</option>
              <option value="medium">Medium</option>
              <option value="large">Large</option>
            </select>
          </div>
          <div class="form-group">
            <strong>Katchup Type</strong>
            <div class="radio" *ngFor="let katchupType of katchupTypes">
              <label for="katchupType">
                <input type="radio" id="katchupType" [value]="katchupType" name="katchup" ngModel> {{ katchupType }}
              </label>
            </div>
          </div>
          <button class="btn btn-primary" type="submit">Order Now</button>
        </div>
      </div>
    </div>
  </form>
</div>
```
Also, we have to add ketchup types array to out typescript code:
```ts
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  ketchupTypes = ['No Katchup', 'Mild', 'Hot'];

  onSubmit(form: NgForm) {
    console.log('Chosen kethup type is: ' + form.value.ketchup);
  }

}
```

After submitting the form, we get the following output in the console:
![pizza-console-radio.png]({{ "/assets/2018-03-01-angular-forms/pizza-console-radio.png" | relative_url }})

### Using Checkbox Element

Check box is another common element we use in the forms, so lets add some to our application. We will use checkboxes to select ingredients for our pizza.

![pizza-gui-checkbox.png]({{ "/assets/2018-03-01-angular-forms/pizza-gui-checkbox.png" | relative_url }})

Lets see the HTML:

```html
<div>
  <form (ngSubmit)="onSubmit(f)" #f="ngForm">
    <div class="container">
      <div class="row justify-content-md-center">
        <div class="col-md-6 ">
          <h1>Welcome to our pizza place!</h1>
          <hr />
        </div>
      </div>
      <div class="row">
        <div class="col-md-6">
          <h3>Create your own pizza!</h3>
          <hr />
          <div class="form-group">
            <label for="name">Pizza Name</label>
            <input type="text" class="form-control" id="name" name="name" ngModel>
          </div>
          <div class="form-group">
            <label for="pizza-size">Choose Size</label>
            <select id="pizza-size" class="form-control" name="pizzaSize" ngModel>
              <option value="small">Small</option>
              <option value="medium">Medium</option>
              <option value="large">Large</option>
            </select>
          </div>
          <div class="form-group">
            <strong>Katchup Type</strong>
            <div class="radio" *ngFor="let ketchupType of ketchupTypes">
              <label for="ketchupType">
                <input type="radio" id="ketchupType" [value]="ketchupType" name="ketchup" ngModel> {{ ketchupType }}
              </label>
            </div>
          </div>
          <div class="form-group">
            <strong>Ingredients</strong>
            <div class="checkbox" *ngFor="let ingredient of ingredients">
              <label>
                <input type="checkbox" (change)="onChange($event)" [name]="ingredient.name">{{ ingredient.name }}</label>
            </div>
          </div>
          <button class="btn btn-primary" type="submit" >Order Now</button>
        </div>
      </div>
    </div>
  </form>
</div>
```

And the typescript:

```ts
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  ketchupTypes = ['No Katchup', 'Mild', 'Hot'];
  ingredients: { name: string, state: boolean }[] = [
    { name: 'Cheese', state: false },
    { name: 'Ham', state: false },
    { name: 'Egg', state: false },
    { name: 'Mushrooms', state: false },
    { name: 'Paprika', state: false },
    { name: 'Olives', state: false }];


  onSubmit(form: NgForm) {
    console.log(this.ingredients);
  }

  onChange(value: any) {
    // Update our ingredients list to keep it in sync with the form's state
    this.updateIngerdientByName(value.target.name, value.target.checked);
  }

  updateIngerdientByName(name: string, newState: boolean) {
    this.ingredients.find(a => a.name === name).state = newState;
  }
}
```

We will go an extra mile for this one, because we want to be able to select multiple ingredients. For this purpose we create a ingredients array it the typescript code, which we will use to track the state of our checkboxes. 
Checkbox states are updated using onChange function, which is bind to checkboxes `change` event. Other than that, we use the same techniques like we did with the other elements.  

Now, when the form is submitted, we can log `ingredients` array to the console, and see that proper ingredients are selected:
![pizza-console-checkbox.png]({{ "/assets/2018-03-01-angular-forms/pizza-console-checkbox.png" | relative_url }})

### Adding Forms Validation

So far we only used _ngForm_ object to access value property, but there is a lot more to it. We can also use this object for the form validation purposes.
Lets say we want to make our pizza name input field mandatory. Here is some code which demonstrates how to achieve this with help of Angular.

```html
          <div class="form-group">
            <label for="name">Pizza Name</label>
            <input type="text" class="form-control" id="name" name="name" required ngModel>
          </div>
```
All we did here is that we have added `required` html validation attribute.



### Grouping Forms Data

### Default Values And Data Binding

### Setting And Patching The Data