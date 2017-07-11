---
layout: post
title:  "Angular 4 Basics"
date:   2017-07-10 11:10:25 +0200
categories: frontend
---

This is absolute beginners guide for setting-up development environment for writing AngularJs 4 application. This post is intended to serve as quick-start guide for developing first Angular 4 project as well as quick reference for Angular basics.
Angular is a JavaScript framework which allows you to create reactive Single-Page-Applications (SPAs). Angular uses Typescript - a super-set to javascript and it offers more features like classes, Types, Interfaces, ...). With Typescript allows you to write more robust code because it is validated at development time and not at the runtime as javascript. Typescript doesn't run in browser, it is compiled to javascript before application runs. This compilation is done by the Angular CLI.

### Content

[Installing NodeJs](#installing-nodejs)

### Requirements

* [NodeJS](https://nodejs.org/en/download/) 
* [Angular CLI](https://cli.angular.io/) *(Not required but highly recommended.)*
* [Visual Studio Code](https://code.visualstudio.com/) (or any text editor)

### Angular CLI

Angular CLI will greatly simplify the process of:
Creating new Angular projects with default settings that are immediately ready to be served
Creating Angular components, routes, services and pipes
Serving your application locally while developing
Running your tests (unit and end-to-end)

#### Creating Angular projects using CLI

Angular projects are created using *new* keyword and specifying name of your project. Just open your command prompt window and run following commands:

```
ng new awesome-app-name
```
This will create new Angular project using default settings at your current location.

#### Creating Angular components, routes, services and pipes
You can take advantage of *generate* command to speed-up creation of Angular building blocks. While in root project location execute following commands:
```
// For creating a component
ng generate component foo
// or just 
ng g c foo
``` 
This will generate folder named *foo* with required html, css, ts and spec files:
* foo
    * foo.component.html
    * foo.component.ts
    * foo.component.css
    * foo.component.spec
 Additionally some code will be added to **app.module.ts** file in order to import newly created component:
 
 ```
 import { BrowserModule } from '@angular/core';
 import { NgModule } from '@angular/core';
 import { AppComponent } from '@angular/core';
 import { FooComponent } from './foo/foo.component';

 @NgModule({
     declarations: [
         AppComponent,
         FooComponent
     ],
     imports: [
         BrowserModule
     ],
     providers: [],
     bootstrap: [AppComponent]
 })
export class AppModule { }
 ```

### Visual Studio Code

### Components

Components are one of the key features of Angular. You generally build your application by composing it from number of components. Every application starts with app-component - a root component of any Angular application. It basically holds the entire application and we nest all our custom components to this component.

### Bindings

Data binding represents way of communication between TypeScript code as your business logic and Template which is visual representation - HTML page that user sees. There are several types of data binding in Angular and which one is used depends of the specific situation. Generally application would use all types of bindings and here they all are: 
String Interpolation ({{ data }}) - a one-way binding used when we want to output some information from our TypeScript code to our Template (HTML). It uses double curly braces with some property or expression in between. It is possible to bind to some TypeScript method or ternary expression as long as it **returns a string value**. It is not possible to write multi-line statements, for example if-else statements. 
```
// String Interpolation example in our HTML code
<p> Hello {{ userName }}!</p>

```
Where *userName* is some property in our TypeScript code for example:
```
import { Component, OnInit } from '@angular/core';

@Component({
  selector: 'app-foo',
  templateUrl: './foo.component.html',
  styleUrls: ['./foo.component.css']
})
export class FooComponent {
  userName = "John";
}
```
Change to the *userName* property would automatically trigger change to the paragraph element by inserting value of *userName*.
Property Binding ([property]="data")
Event Binding((event)="expression")
Two-Way-Binding ([(ngModel)]="data")

### Directives

#### Built-in directives

#### Custom directives


