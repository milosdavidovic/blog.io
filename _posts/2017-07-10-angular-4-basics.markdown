---
layout: post
title:  "Angular 4 Basics"
date:   2017-07-10 11:10:25 +0200
categories: frontend
---

This is absolute beginners guide for setting-up development environment for writing AngularJs 4 application. This post is intended to serve as quick-start guide for developing first Angular 4 project as well as quick reference for Angular basics.

### Content

[Installing NodeJs](#installing-nodejs)

### Requirements

* [NodeJS](https://nodejs.org/en/download/) 
* [Angular CLI](https://cli.angular.io/) *(Not required but highly recommended.)*
* [Visual Studio Code](https://code.visualstudio.com/) (or any text editor)

### Installing NodeJs

### Installing Angular CLI

### Installing Visual Studio Code

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

### Bindings

### Directives

#### Built-in directives

#### Custom directives


