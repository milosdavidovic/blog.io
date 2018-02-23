---
layout: post
title:  "Angular Routing"
date:   2018-02-21 16:27:25 +0100
categories: frontend
published: true
---

For navigating inside our Angular applications we usually don't want to force a browser reload, because we want to keep our application's state. This is where the Angular Routing comes to aid. It provides the mechanisms to achieve just that - navigating inside our application, without triggering page reload. This also helps to organize our applications better.


## Content

[Adding Router Paths](#adding-router-paths)

[Loading a Component](#loading-a-component)

[Programmatic Approach](#programmatic-approach)

[Navigation With Router Links](#navigation-with-router-links)

[URL Parameters](#url-parameters)

[Wild Card Routes](#wild-card-routes)

[Retrieving Query Parameters](#retrieving-querry-parameters)

### Adding Router Paths

Create a object of type Routes which will hold an array all routes supported by the application. The Routes class should be imported from @angular/router.

#### Routes declaration

```typescript
const appRoutes: Routes = [
    { path: '', component: HomeComponent },
    { path: 'users', component: UsersComponent }
];
```

Here we are defining a two root paths: ' ' (empty or default path) and 'users' path and the code is pretty straightforward.
If we have our application served locally on the port 4200 for example, visiting the address http://localhost:4200 would load our HomeComponent, while visiting http://localhost:4200/users would load the UsersComponent. We could say that our HomeComponent is serving like a default one in this setup.

_NOTE: We need to place `<router-outlet></router-outlet>` tag in out app.componenet.html to serve as a placeholder for loading a components based on the route._

#### Child Routes

```typescript
const appRoutes: Routes = [
    { path: '', HomeComponent },
    { path: 'home', component: HomeComponent },
    { path: 'users', component: UsersComponent, children: [
        { path: 'new', component: CreateUserComponent },
        { path: 'edit', component: EditUserComponent }
    ]
];
```

With this we defined two new routes in our application:  
`http://localhost:4200/users/new` - which will load the CreateUserComponent
`http://localhost:4200/users/edit` - which will load the EditUserComponent

But how does the Angular knows where to load these child components? For this to work we need to add another `<router-outlet></router-outlet>` placeholder, but this time not in our app.component.html, but instead in our parent component. In this case it is a UsersComponent.

#### Redirecting

```typescript
const appRoutes: Routes = [
    { path: '', redirectTo: '/home', pathMatch: 'full' },
    { path: 'home', component: HomeComponent },
    { path: 'users', component: UsersComponent, children: [
        { path: 'new', component: CreateUserComponent },
        { path: 'edit', component: EditUserComponent }
    ]
];
```

In this setup we still have two main paths 'home' and 'users' but we are using redirectTo property to set redirect user to another path instead loading a specific component.  

> _NOTE: We are using pathMatch parameter to set matching strategy for this path. By default, Angular matches paths by prefix. For our example this means that all paths starting with /home would be redirected to the 'home' path. If this is not a desired behavior we could set matching strategy to 'full' and the redirection will happen only in case of exact match._

#### Passing Parameters to Routes

```ts
const appRoutes: Routes = [
    { path: '', redirectTo: '/home', pathMatch: 'full' },
    { path: 'home', component: HomeComponent },
    { path: 'users', component: UsersComponent, children: [
        { path: 'new', component: CreateUserComponent },
        { path: 'edit', component: EditUserComponent },
        { path: ':id', component: UserComponent }
    ]}
];
```

What if we need for our route to also have some dynamic data, not just hardcoded strings? We can achieve this by injecting parameters to our route using the column followed with the parameter name, like `:id`.  
If we would now be able to load UserComponent and passing `1` as a parameter, by visiting the route:  
`http://localhost:4200/users/1`

#### Retrieving Route Parameters

```ts
// ActivatedRoute need to be imported from @angular/router
// import { ActivatedRoute } from '@angular/router';

let id = this.activatedRoute.snapshot.params['id'];
```

This way we are retrieving parameter named _id_ from our route and setting it's value to the local variable.

If there is a chance that parameter will change while we have the same component loaded and we what to store the latest value in our id variable, we need to subscribe to the params observable:

 ```ts
// ActivatedRoute need to be imported from @angular/router
// import { ActivatedRoute } from '@angular/router';
private id: any;

this.activatedRoute.params
    .subscribe(
        (params: Params) => {
            this.id = params['id'];
        }
    );
```

### Navigation

When we have out Routes configured, we can use them and navigate inside the our application. 

#### Navigation With Router Links

```html
<ul class="dropdown-menu">
    <li>
        <a [routerLink]="['new']" [queryParams]="{bar: foo}" routerLinkActive="active">Add User</a>
    </li>
    <li>
        <a [routerLink]="['edit']" routerLinkActive="active">Edit User</a>
    </li>
</ul>
```

**routerLink** directive here to define the navigation path. It is a array where we can use static strings and our variables from to construct the desired path.  
First element of the array routerLink array can be prefixed to fine-tune behavior when constructing a path:

* 'users' - path we have defined will be appended to the current location;
* './users'_ - this will result in the same behavior like above;
* '/users'_ - path we have defined will be appended to the route path of the our application;
* '../users'_ - router will go up one level and then append path we have defined to the current location;

**queryParams** directive can be used to append query parameters to the route link and they will ne appended after '?' sign, for example:  
`http://localhost:4200/users/new?foo=bar`

**routerLinkActive** directive allows us to set css class to an element when the link's route becomes active. This is useful when creating menus or a navigation for example.

#### Programmatic Approach

```ts
// Router and ActivatedRoute need to be imported from @angular/router
// import { ActivatedRoute, Router } from '@angular/router';

this.router.navigate(['new', user.id], { relativeTo: this.activeRoute, queryParams: {foo: 'bar'} });
```

Here we are providing segments of the route link we want to navigate to to the router's navigate method as an array. First element of the array routerLink array can be prefixed, same way we did when using routerLink in our html.
Second argument of the navigate method represent _navigation extras_ and we can use it to further define the navigation. When we set relativeTo property to activeRoute, we are instructing a router to append path we defined in navigate method to the current link route.
We use queryParams property to add query parameters, same way we did when using routerLink in our html.

#### Wild Card Routes
Sometimes we don't want to be so specific about the route we are defining, to be able to cover some range of routes. Example for this might be the _404 Page Not Found_ page, when user hits the route we did not explicitly defined in our application.

```ts
const appRoutes: Routes = [
    { path: '', redirectTo: '/home', pathMatch: 'full' },
    { path: 'home', component: HomeComponent },
    { path: 'users', component: UsersComponent, children: [
        { path: 'new', component: CreateUserComponent },
        { path: 'edit', component: EditUserComponent }
    ],
    { path:'not-found', component: PageNotFoundComponent },
    { path:'**', redirectTo: '/not-found' }
}];
```

Here we use double asterisk (`**`) wildcard character to "catch" all routes we are not supporting and re

>_NOTE: Defined routes are parsed form top to bottom, so it is a good idea to place generic routes like this one at the end. If we have had defined this '**' path first, we would be always redirected to the not-found root, no matter which route we visit._ 

#### Retrieving Query Parameters

```ts
// ActivatedRoute need to be imported from @angular/router
// import { ActivatedRoute } from '@angular/router';

let foo = this.activatedRoute.snapshot.queryParams['foo'];
```

This way we are retrieving query parameter named foo from out route and setting it's value to the local variable.

If there is a chance that query parameter will change while we have the same component loaded and we what to store the latest value in our foo variable, we need to subscribe to the queryParameter observable:

 ```ts
// ActivatedRoute need to be imported from @angular/router
// import { ActivatedRoute } from '@angular/router';
private foo: any;

this.activatedRoute.queryParams
    .subscribe(
        (queryParams: Params) => {
            this.foo = queryParams['foo'];
        }
    );
```