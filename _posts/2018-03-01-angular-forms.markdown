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

This is how our example form looks like:
//TODO add photo

Let's create some basic HTML for demonstration purposes:

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
        </div>
      </div>
    </div>
  </form>
</div>
```

And the appropriate typescript to go with it:

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

![My helpful screenshot]({{ "/assets/2018-03-01-angular-forms/pizza-gui-1.png" | relative_url }})

But to understand how this works, let examine what Angular exactly did for us here. We have our HTML form element, but it has some additional Angular syntax:  
`<form (ngSubmit)="onSubmit(f)" #pizzaForm="ngForm">`  
The `ngSubmit` is a directive which specifies the function to be executed when the form is submitted. In the example we have specified that onSubmit function should be executed and this function is defined in our typescript.  
What is interesting is that we were able to pass parameter f to this function, which is of type NgForm. NgForm is a another directive, and we can export a directive into a local template variable like: `#pizzaForm="ngForm"`.  
But this is just one peace of the puzzle.

`<input type="text" class="form-control" id="name" name="name" ngModel>`  
To finally get access to form values, we need to use another directive - `ngModel`, to tell Angular in what elements of the form are we interested in. also, we need to give name to our element using the name attribute, and this name we will also use when accessing the elements value in our typescript code.  
Finally we are able to get value from the input element as follows:  
`console.log('Value of a name variable is: ' + form.value.name);`



[Accessing Forms Data From Typescript](#accessing-forms-data-from-typescript)

[Grouping Forms Data](#grouping-forms-data)

[Adding Forms Validation](#adding-forms-validation)

[Default Values And Data Binding](#default-values-and-data-binding)

[Using Radio Buttons](#using-radio-buttons)

[Using Select Element](#using-select-element)

[Using Checkbox Element](#using-checkbox-element)

[Setting And Patching The Data](#setting-and-patching-the-data)

### Creating Simple Form

### Accessing Forms Data From Typescript

### Grouping Forms Data

### Adding Forms Validation

### Default Values And Data Binding

### Using Radio Buttons

### Using Select Element

### Using Checkbox Element

### Setting And Patching The Data