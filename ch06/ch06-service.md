# Service

The components are using static fake data to display heroes. In real world, you should use services to get data from a web server.

## 1 Create the HeroService

As before, you use Angular CLI to create starting code of a service: `ng generate service hero`. The command create a `HeroService` class in `src/app/hero.service.ts` file.

Dependency injection allows you to use a class without explicitly creating an instance of the class. To let the dependency injection (DI) system know a service, you need to register a `provider` -- a configuration that specifies how the DI create an instance of a class.

In this example, the service class comes with an `@Injectable({ provideIn: 'root'})` decorator. It is used by the Angular dependency injection system. The decorator means that an instance of the `HeroService` class can be injected into your application at the root level. Angular creates a single root-level service instance that is shared by all components that uses it.

Usually a service like this get data from a web service, local storage, or, often in testing, from a mock data source. In this example, it still returns the mock data.

```ts
import { Injectable } from '@angular/core'
import { Hero } from './hero'
import { HEROES } from './mock-heroes'

@Injectable({
  providedIn: 'root',
})
export class HeroService {
  constructor() {}

  getHeroes(): Hero[] {
    return HEROES
  }
}
```

## 2 Inject the Service

Angular uses constructor injection that provides a `HeroService` parameter in the component class constructor to get it injected: `constructor(private heroService: HeroService) { }`. After this, you can use the `this.heroService` anywhere in the component.

Create a method to use the service in the component class as the following:

```ts
getHeroes(): void {
  this.heroes = this.heroService.getHeroes();
}
```

It's a best practice that the component class constructor is primarily used for dependency injection. It usually doesn't perform other functions, especially not to call a web service or to load data from the local storage.

The recommended place to load initial data is in a lifecycle hook method called `ngOnInit()`. During the component intialization, Angular will call this method to perform initialization tasks such as data loding. The method is defined in `OnInit` interface that the component class should inherit from. The code is as the following:

```ts
export class HeroesComponent implements OnInit {
  heroes: Hero[]
  constructor(private heroService: HeroService) {}

  ngOnInit() {
    this.getHeroes()
  }
}
```

Now you should be able to run the application and see the list of heros.

## 3 Observable Data

In the above example, the hero service returns the predefined mock data immediately. In real world, the data is retrieved in asynchornous mode: it takes time to get the data. You shouldn't wait for the operation becaues it hurts the user experience. Let's use the async RxJS library to simulate the real scenario.

In RxJS, asynchronous calls return an `Observable` data stream that the caller can subscribe to the data when it becomes available. Use the following code to mock an async operation:

```ts
import { Observable, of } from 'rxjs'

// ... other code
// in the service class
export class HeroService {
  getHeroes(): Observable<Hero[]> {
    return of(HEROES)
  }
}
```

The RxJS `of` method converts the static `HEROS` data into an observable stream data. The `Observable<Hero[]>` is a type declartion: the data is an `Observable` that will emit an array of heros `Hero[]`.

To use the data, subscribe to the observable in the heroes component class as the following:

```ts
getHeroes(): void {
  Observable<Hero[]> heors$ = this.heroService.getHeroes();
  heros$.subscribe(heroes => this.heroes = heroes);
}
```

The `$` suffix is a regular character in a variable name. It's a convention that you use it to name an `Observable` varaible.

the `susbscribe` method takes a function as its parameter. `heroes => this.heroes = heroes` is an arrow function that takes one parameter that is the observable data. When the data is ready, this function is called.

Now the appliation works in an asynchronous way.

## 4 Show Messages

In a real application, usually there are serveral services working together. Now create a message component to demostrate the service usages.

### 4.1 Create a New Component

As before, use the CLI to create a new component called `MessagesComponent` using the command `ng generate component messages`.

### 4.2 Display the New Component

Then modify the `src/app/app.component.html` file to display the view of the messages component in the root component.

```html
<h1>{{title}}</h1>
<app-heroes></app-heroes>
<app-messages></app-messages>
```

### 4.3 Create a Message Service

Use the `ng g service message` to create a service file `src/app/message.service.ts` and edit it to have the following content:

```ts
import { Injectable } from '@angular/core'

@Injectable({
  providedIn: 'root',
})
export class MessageService {
  messages: string[] = []
  add(message: string) {
    this.messages.push(message)
  }
  clear() {
    this.messages = []
  }
}
```

### 4.4 Use the Message Service

Import and use the message service in the heros service.

```ts
import { Injectable } from '@angular/core'
import { Observable, of } from 'rxjs'
import { Hero } from './hero'
import { HEROES } from './mock-heroes'
import { MessageService } from './message.service'

@Injectable({
  providedIn: 'root',
})
export class HeroService {
  constructor(private messageService: MessageService) {}

  getHeroes(): Observable<Hero[]> {
    // TODO: send the message _after_ fetching the heroes
    this.messageService.add('HeroService: fetched heroes')
    return of(HEROES)
  }
}
```

Like a component, a service can inject another service using a constuctor paramter.

### 4.5 Display the Messages

As before, inject the messages service in messages component class contructor and edit the html template to display the messages.

The `src/app/messages/messages.component.ts` has the following content:

```ts
import { Component, OnInit } from '@angular/core'
import { MessageService } from '../message.service'

@Component({
  selector: 'app-messages',
  templateUrl: './messages.component.html',
  styleUrls: ['./messages.component.css'],
})
export class MessagesComponent implements OnInit {
  constructor(public messageService: MessageService) {}
  ngOnInit() {}
}
```

The `src/app/messages/messages.component.html` has the following content:

```html
<div *ngIf="messageService.messages.length">

  <h2>Messages</h2>
  <button class="clear"
          (click)="messageService.clear()">clear</button>
  <div *ngFor='let message of messageService.messages'>
    {{message}}
  </div>
</div>
```
