---
layout: post
title:  "Angular Forms - The Template Approach"
date:   2018-03-01 16:27:25 +0100
categories: frontend
published: false
---

It is absolutely possible to create form and handle user input in the HTML, why would we need Angular's help to do this? Problem lies in the fact that we usually end up with submitting form's data to the server using the submit button. Due to the fact that we are creating SPA with help of the Angular framework and therefore there no "submitting", we need a different approach. Instead of the submitting, we can use Angular's Http Module to send form's data back and forth between our application and some backend. Additionally, Angular helps us with this task by providing lot of functionality one might need when dealing with forms. Some of this functionality we get out-of-box with Angular are: easy access to form's data, user input validation, conditionally changes how form is displayed and what is visible or enabled and much more. 
There are two approaches to developing forms in Angular: Template and Reactive. This post will cover some of the basics of the Template approach through an example of creating simple pizza ordering form ('cuz everyone likes pizza!).

## Content

[Creating Simple Form](#creating-simple-form)

This is how our example form looks like:
//TODO add photo

And here is the HTML:

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
        
        <div class="col-md-3">
          <h3>Choose your pizza!</h3>
          <hr />
          <div class="form-group">
            <label for="pizza">Choose type</label>
            <select id="pizza" class="form-control" name="pizza" [ngModel]="'neapolitan'">
              <option value="neapolitan">Neapolitan Pizza</option>
              <option value="margherita">Margherita Pizza</option>
              <option value="greek">Greek Pizza</option>
            </select>
          </div>
          <div class="form-group">
            <label for="radio-wrapper" style="margin-bottom:-5px">Katchup Type</label>
            <div id="radio-wrapper" style="margin-top:-5px">
              <div class="radio" *ngFor="let katchupType of katchupTypes">
                <label for="katchupType">
                  <input type="radio" id="katchupType" [value]="katchupType" name="katchup" [ngModel]="katchupTypes[0]"> {{ katchupType }}
                </label>
              </div>
            </div>
          </div>
          <div ngModelGroup="extras">
            <div class="form-group">
              <label for="checkbox-wrapper" style="margin-bottom:-5px">Extras</label>
              <div id="checkbox-wrapper" style="margin-top:-5px">
                <div class="checkbox" *ngFor="let ex of extras">
                  <label>
                    <input type="checkbox" [value]="ex" [name]="ex" [ngModel]="extrasInitialValue">{{ ex }}</label>
                </div>
              </div>
            </div>
          </div>
          <div class="form-group">
            <label for="note">Additional note</label>
            <textarea class="form-control" id="note" name="note" ngModel></textarea>
          </div>
        </div>

        <div class="col-md-3">
          <h3>Delivery Details</h3>
          <hr />
          <div ngModelGroup="address-group" #addressGroup="ngModelGroup">
            <div class="form-group">
              <label for="city">Choose city</label>
              <select class="form-control" name="city" id="city" [ngModel]="'Boston'">
                <option value="Boston">Boston</option>
                <option value="New York">New York</option>
              </select>
            </div>
            <div class="form-group">
              <label for="address">Address</label>
              <input type="text" class="form-control" id="address" name="address" ngModel required>
            </div>
            <div class="form-group">
              <label for="phone">Phone</label>
              <input type="text" class="form-control" id="phone" name="phone" ngModel required>
            </div>
          </div>
          <p *ngIf="!addressGroup.valid && addressGroup.touched" class="text-danger">Please fill in the address details.</p>
          <button class="btn btn-primary" type="submit" [disabled]="!f.valid">Order Now</button>
        </div>
      </div>
    </div>
  </form>
</div>
```

And appropriate typescript:

```ts
import { Component } from '@angular/core';
import { NgForm } from '@angular/forms';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
  katchupTypes = ['No Katchup', 'Mild', 'Hot'];
  extras = ['Extra cheese', 'Olives'];
  extrasInitialValue = false;
  initialNote = 'You can write additional notes here...';

  onSubmit(form: NgForm) {
    console.log(form);
  }
}

```

[Accessing Forms Data From Typescript](#accessing-forms-data-from-typescript)

[Grouping Forms Data](#grouping-forms-data)

[Adding Forms Validation](#adding-forms-validation)

[Default Values And Data Binding](#default-values-and-data-binding)

[Using Radio Buttons](#using-radio-buttons)

[Using Select Element](#using-select-element)

[Setting And Patching The Data](#setting-and-patching-the-data)

### Creating Simple Form

### Accessing Forms Data From Typescript

### Grouping Forms Data

### Adding Forms Validation

### Default Values And Data Binding

### Using Radio Buttons

### Using Select Element

### Setting And Patching The Data