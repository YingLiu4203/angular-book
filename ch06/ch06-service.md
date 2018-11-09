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
  const heors$ = this.heroService.getHeroes();
  heros$.subscribe(heroes => this.heroes = heroes)
}
```

The `$` suffix is a regular character in a variable name. It's a convention that you use it to name an `Observable` varaible.

the `susbscribe` method takes a function as its parameter. `heroes => this.heroes = heroes` is an arrow function that takes one parameter that is the observable data. When the data is ready, this function is called.

Now the appliation works in an asynchronous way.
