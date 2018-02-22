---
layout: post
title:  "Angular Routing"
date:   2018-02-21 16:27:25 +0100
categories: frontend
published: true
---

For navigating inside our Angular applications we usually don't want to force browser reload, because we want to keep our application's state. This is where Angular Routing comes to aid. It provides mechanisms to achieve just that - navigating our application without reloading the page and to help organize our applications better.



### Content

[Adding Router Paths](#adding-router-paths)

[Loading a Component](#loading-a-component)

[Programmatic Approach](#programmatic-approach)

[Navigation With Router Links](#navigation-with-router-links)

[URL Parameters](#url-parameters)

### Adding Router Paths

Create object of type Routes which will hold an array all routes supported by the application. Routes class should be imported from @angular/router.

#### Routes declaration

```typescript
const appRoutes: Routes = [
    { path: '', component: HomeComponent },
    { path: 'users', component: UsersComponent }
];
```

Here we are defining a two root paths: ' ' (empty or default path) and 'users' path and code is pretty straightforward.
If we have our application served locally on the port 4200 for example, visiting the address http://localhost:4200 would load our HomeComponent, while visiting http://localhost:4200/users would load UsersComponent. We could say that our HomeComponent is serving like a default one in this setup.

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
}];
```

With this we defined two new routes in our application:  
`http://localhost:4200/users/new` - which will load the CreateUserComponent
`http://localhost:4200/users/edit` - which will load the EditUserComponent

But how does Angular knows where to load these child components? For this to work we need to add another `<router-outlet></router-outlet>` placeholder, but this time not in out app.component.html, but instead in our parent component. In this case it is a UsersComponent.

#### Redirecting

```typescript
const appRoutes: Routes = [
    { path: '', redirectTo: '/home', pathMatch: 'full' },
    { path: 'home', component: HomeComponent },
    { path: 'users', component: UsersComponent, children: [
        { path: 'new', component: CreateUserComponent },
        { path: 'edit', component: EditUserComponent }
    ]
}];
```

In this setup we still have two main paths 'home' and 'users' but we are using redirectTo property to set redirect user to another path instead loading a specific component.  

_NOTE: We are using pathMatch parameter to set matching strategy for this path. By default, Angular matches paths by prefix. For our example this means that all paths starting with /home would be redirected to the 'home' path. If this is not a desired behavior we could set matching strategy to 'full' and the redirection will happen only in case of exact match.

### Navigation

When we have out Routes configured, it is time to use them and navigate inside our application. 

#### Programmatic Approach

```ts
// Router and ActivatedRoute need to be imported from @angular/router
// import { ActivatedRoute, Router } from '@angular/router';

this.router.navigate(['edit'], { relativeTo: this.activeRoute });
```

#### Navigation With Router Links

```html
<ul class="dropdown-menu">
    <li>
        <a style="cursor: pointer" [routerLink]="['new']" routerLinkActive="active">Add User</a>
    </li>
    <li>
        <a style="cursor: pointer" [routerLink]="['edit']" routerLinkActive="active">Edit User</a>
    </li>
</ul>
```

### URL Parameters
/TODO
