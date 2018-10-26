# Chapter 7 Routing

This chapter introduces the Angular routing mechanism. The app now has two views: the existing heros view and a dashboard view. The hero detail views is used as a child in both top level views.

## 1 Add Routing Module

All Angular functions are provided via a module. The Angular routing functions of an application are usually defined in one or more so-called routing modules.

Use the CLI command to generate the root routing module: `ng generate module app-routing --flat --module=app`. The `--flat` option tells the CLI to generate the module file in the `src/app` folder, not in its own folder. The `--module=app` register the new routing module in the `AppModule`. You can use any valid module name for the routing module. However, `app-routing` makes it clear that this is the root level module. The generate file `src/app/app-routing.ts` has the following content:

```ts
import { NgModule } from '@angular/core'
import { CommonModule } from '@angular/common'

@NgModule({
  imports: [CommonModule],
  declarations: [],
})
export class AppRoutingModule {}
```

The `CommonModule` has common directives such as `nfIf`, `ngFor` etc. used by most components. However, you shouldn't define any components in a routing moudle. Therefore you can delete the `imports` and `declarations` in the `NgModule` decorator.

The routing module uses the `Routes` type defined in Angular `RouterModule`. Also, to make the router directives available to all components, the routing module exports the `RouterModule`. The changed code look like the following:

```ts
import { NgModule } from '@angular/core'
import { Routes, RouterModule } from '@angular/router'

@NgModule({
  exports: [RouterModule],
})
export class AppRoutingModule {}
```

## 2 Add Routes

There is a router service provided by the `RouterModule` that controls all route navigation. Routes tell the router service which view (consisting of one or more components) to display when the url changes, often as a result of clicking a link or manual change of the url in the browser address bar.

A route is defined by a path and a component. The `Routes` type is an array of routes. The heros component can be defined in this route: `{ path: 'heroes', component: HeroesComponent }`. The path in url is `/heroes` and the component to display is the`HeroesComponent`. Edit the `src/app/app-routing.ts` to have the following content:

```ts
import { NgModule } from '@angular/core'
import { Routes, RouterModule } from '@angular/router'
import { HeroesComponent } from './heroes/heroes.component'

const routes: Routes = [{ path: 'heroes', component: HeroesComponent }]

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule],
})
export class AppRoutingModule {}
```

You should call `RouterModule.forRoot(routes)` for the applicaton root level routes. In one application, it can be called once in the root module. You can find it is in the array of the `imports` property of the `@NgModule` decorator of the `AppModule`.

## 3 Navigate to Heroes

To display a component when its path matches in an url, use the `router-outlet` element tag. It is provided by the `RouterModule`. Change the `src/app/app.component.html` to have the following content:

```html
<h1>{{title}}</h1>
<nav>
  <a routerLink="/heroes">Heroes</a>
</nav>
<router-outlet></router-outlet>
<app-messages></app-messages>
```

Now the home page `/` only has a title. When you change the url to `/heroes`, you can see the list of heroes.

You can click the link to navigate to the heroes view too. The `routerLink` attribute, again a directive provided by the `RouterModule`, sets the link url to its value when clicked.

## 4 Add a Dashboard View

Now add a new component `DshboardComponent` that display top 5 heroes from the heroes list. You also want to make it as the default route.

Now the routes has the following content:

```ts
const routes: Routes = [
  { path: '', redirectTo: '/dashboard', pathMatch: 'full' },
  { path: 'dashboard', component: DashboardComponent },
  { path: 'heroes', component: HeroesComponent },
]
```

The first path is the default path that matches `/`, the root url path. The `pathMatch: 'full'` means that it cannot have anything after the `'/'` because by default the empty string `''` matches any path as its prefix or suffix.

## 5 Add a Detail View

## 6 Get Router Parameter

## 7 Find a Way Back
