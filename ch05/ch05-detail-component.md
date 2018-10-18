# Chapter 5: Detail Component

Currently the `HeroesComponent` displays both the list of heros and the deatails of a hero. Not only the component is bigger, but also it is hard to reuse. You may want to display hero details in other places but it is part of another component.

Once the problem is identified, the solution is straightforward: just put the hero details into a new component.

## 1 Create a New Component

As before, use Angular CLI to generate a component: `ng generate component hero-detail`. The command creates a `src/app/hero-detail` folder and the three files of the new component: the class file `hero-detail.component.ts`, the html file `hero-detail.component.html`, and the style file `hero-detail.component.css`.

## 2 Add an Input

To display the details of a hero, the hero detail component needs to take a hero as its input. Angular uses `@Input()` to mark a class property as the input of the component.

The `@Input()` is called a decorator. All decorators have a `@` prefix to distinguish it from other classes/functions. The `hero-detail.component.ts` has the following content:

```ts
import { Component, OnInit, Input } from '@angular/core'
import { Hero } from '../hero'

@Component({
  selector: 'app-hero-detail',
  templateUrl: './hero-detail.component.html',
  styleUrls: ['./hero-detail.component.css'],
})
export class HeroDetailComponent implements OnInit {
  @Input()
  hero: Hero

  constructor() {}

  ngOnInit() {}
}
```

## 3 Write the HTML Template

You cut the hero detail elements from the `HeroesComponent` and paste it to the `HeroDetailComponent`. Becuase only one hero is displayed in the hero detail component, revise the html template as the following:

```html
<div *ngIf="hero">
  <h2>{{hero.name | uppercase}} Details</h2>
  <div><span>id: </span>{{hero.id}}</div>
  <div>
    <label>name:
      <input [(ngModel)]="hero.name" placeholder="name"/>
    </label>
  </div>
</div>
```

## 4 Use the Component

To use the new component, it is similar to use an html element. Remove hero detail elements from the `heroes.component.html` and add this line: `<app-hero-detail [hero]="selectedHero"></app-hero-detail>`.

The `app-hero-detail` is the value of the `selector` metadata of the hero detail component. The `[hero]` means that the attribute value of `hero`, i.e., the `"selectedHero"` should be evaluated as a TS expression. The square brackets `[]` around an element attribute means that the attribute value is a TS expression, not a regular string. The value is TS code to be evaluated. In this example, the `selectedHero` is a property of heros component and its value is the current selected hero.

There is nothing changed in the UI but you have a reusable hero detail component that can be used in other places.

## 5 [], (), and [()]

You have seen all three types of symbols around an elment attribute.

The square bracket `[someAttribute]="something"` means that the attribute `someAttribute`'s value, `someting`, is a TS expression that will be evaluated and its result will be used as the value of `someAttribute`. To compare this with the version `someAttribute="something"` that doesn't have square bracket, the `someAttribute` is assigned a value of `something`, just a regular HTML attribute assignment.

The parenthesis `(someEvent)="eventListener"` is used to define an event listner.

The box of bananas `[()]` defines a two-way binding between an attribute and a component property.
