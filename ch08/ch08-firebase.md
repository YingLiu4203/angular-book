# Install and Configure Firebase

This chapter gives a step-by-step tutorial on Firebase installation and configuration. [Firebase](https://firebase.google.com/) is a Backen-as-a-service (BaaS) that provides many functions such as authentication/storage/tracing etc for web and mobile application development.

## 1 Setup Firebase

First create an account in Firebase. It has a free plan that is enough for learning purpose. Go to the [Firebase console](https://firebase.google.com/), click `Add project`, give it a name such as `is445-demo-1127` (feel free to use whatever name you like), uncheck the Google Analytics option, click `Countinue` and click `Create Project` in the following page. Firebase will create your project in less than a minute.

![Adding Firebase to your app](./firebase-01.png)

Click the `Database` on the left menu and click `Create database`.

![Create database](./firebase-02.png)

Click the `Start in test mode` and click `Enable`.

![Start in test mode](./firebase-03.png)

Click `Add Collection` button to add a new collection with a name of `books`.

![Add Collection](./firebase-04.png)

And add three strin fields: `author`, `description` and `title`. Click the `Save` button.
![Add Fields](./firebase-05.png)

Now the data should have a `books` collection with three fields.

![Collection and Fields](./firebase-06.png)

## 2 Init Angular Project

First create an empty github project (no README, no license) using any name you like. In my case, it is [firebase-demo](https://github.com/csulbying/firebase-demo).

![Empty repository](./github-01.png)

Then in your computer create a new Angular project using `ng new firebase-demo`. Just press `Enter` to accept the default options.

Run `cd firebase-demo` to go to the newly created project folder and run `ng serve -o` to test that it runs as expected. If it works, commit the changes using command `git commit -am

Then run `git remote add origin your-repository-path` to set the remote repository as the new repository you just created. Make sure to use your repository path, in my case it's `https://github.com/csulbying/firebase-demo.git` as shown in the empty repository picture.

Then commit and push the initial project code using `git push -u origin master`.

## 3 Install and Configure Firebase SDK

[AngularFire](https://github.com/angular/angularfire2) is the official library to use Firebase in an Angular application. [The installation and setup doc](https://github.com/angular/angularfire2/blob/master/docs/install-and-setup.md) has the detail information to install and setup the library.

First install Firebase and AngularFire: `npm install @angular/fire firebase --save`.

Then click the `</>` (the web icon) in your project overview to get the Firebase configuration like the following.

![Firebase Configuration](./firebase-07.png)

Put the configuration in `src/environments/environment.ts` file as the following. Yours should be different because you have a different project configuration.

```ts
export const environment = {
  production: false,
  firebase: {
    apiKey: 'AIzaSyCITp9V2-ym6EpcC6BkkAl_2jl_rmOHRRg',
    authDomain: 'is445-demo-1127.firebaseapp.com',
    databaseURL: 'https://is445-demo-1127.firebaseio.com',
    projectId: 'is445-demo-1127',
    storageBucket: 'is445-demo-1127.appspot.com',
    messagingSenderId: '63428111855',
  },
}
```

Also put the same firebase configuration in `src/environments/environment.prod.ts`. The first line in the production envionrmentis is the `production` property. It should be true: `production: true`.

Then add your Firebase configuration and Firestore module in the root module `/src/app/app.module.ts`. It should have the following content:

```ts
import { BrowserModule } from '@angular/platform-browser'
import { NgModule } from '@angular/core'

import { AppComponent } from './app.component'

import { AngularFireModule } from '@angular/fire'
import { AngularFirestoreModule } from '@angular/fire/firestore'
import { environment } from '../environments/environment'

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    AngularFireModule.initializeApp(environment.firebase),
    AngularFirestoreModule,
  ],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

After this, the `AngularFirestore` is available by DI (dependency injection) to all components.

## 4 Create Routes

First create four new components for CRUD using Angular CLI:

```sh
ng g c boards
ng g c boards-detail
ng g c boards-create
ng g c boards-edit
```

Then define the routes for these components in `src/app/app.module.ts` as the following:

```ts
import { BrowserModule } from '@angular/platform-browser'
import { NgModule } from '@angular/core'
import { RouterModule, Routes } from '@angular/router'

import { AppComponent } from './app.component'

import { AngularFireModule } from '@angular/fire'
import { AngularFirestoreModule } from '@angular/fire/firestore'
import { environment } from '../environments/environment'
import { BoardsComponent } from './boards/boards.component'
import { BoardsDetailComponent } from './boards-detail/boards-detail.component'
import { BoardsCreateComponent } from './boards-create/boards-create.component'
import { BoardsEditComponent } from './boards-edit/boards-edit.component'

const appRoutes: Routes = [
  {
    path: 'boards',
    component: BoardsComponent,
    data: { title: 'Boards List' },
  },
  {
    path: 'boards-details/:id',
    component: BoardsDetailComponent,
    data: { title: 'Boards Details' },
  },
  {
    path: 'boards-create',
    component: BoardsCreateComponent,
    data: { title: 'Create Boards' },
  },
  {
    path: 'boards-edit/:id',
    component: BoardsEditComponent,
    data: { title: 'Edit Boards' },
  },
  { path: '', redirectTo: '/boards', pathMatch: 'full' },
]

@NgModule({
  declarations: [
    AppComponent,
    BoardsComponent,
    BoardsDetailComponent,
    BoardsCreateComponent,
    BoardsEditComponent,
  ],
  imports: [
    BrowserModule,
    RouterModule.forRoot(appRoutes),
    AngularFireModule.initializeApp(environment.firebase),
    AngularFirestoreModule,
  ],
  providers: [],
  bootstrap: [AppComponent],
})
export class AppModule {}
```

Change the `src/app/app.component.html` to use the routes.

```html
<div style="text-align:center"><h1>Welcome to Angular Firebase!</h1></div>

<router-outlet></router-outlet>
```

## 5 Install and Configure Angular Material

[Angular Material](https://material.angular.io/) is a component library created by Angular team.

First install Angular Material using Angular CLI: `ng add @angular/material`.

Then imports material modules used by your application in `src/app/app.module.ts`:

```ts
// Angular Material needs this
import { BrowserAnimationsModule } from '@angular/platform-browser/animations'

import {
  MatInputModule,
  MatPaginatorModule,
  MatProgressSpinnerModule,
  MatSortModule,
  MatTableModule,
  MatIconModule,
  MatButtonModule,
  MatCardModule,
  MatFormFieldModule,
} from '@angular/material'

import { ReactiveFormsModule } from '@angular/forms'
```

Import reactive forms module `import { ReactiveFormsModule }` from '@angular/forms'`.

Then register above modules in the `@NgModule` imports. It has the following content:

```ts
imports: [
    BrowserModule,
    RouterModule.forRoot(appRoutes),
    AngularFireModule.initializeApp(environment.firebase),
    AngularFirestoreModule,
    ReactiveFormsModule,
    BrowserAnimationsModule,
    MatInputModule,
    MatTableModule,
    MatPaginatorModule,
    MatSortModule,
    MatProgressSpinnerModule,
    MatIconModule,
    MatButtonModule,
    MatCardModule,
    MatFormFieldModule,
  ],
```

## 6 The Firebase Services

First you should create a service to implement the CRUD functions that manipulate the Firestore - a Firebase database.

Use CLI command `ng g service firestore` and edit it to have the following code:

```ts
import { Injectable } from '@angular/core'

import { Observable, from } from 'rxjs'
import { map } from 'rxjs/operators'

import { AngularFirestore, AngularFirestoreCollection } from '@angular/fire/firestore'

export interface Board {
  id?: string
  author: string
  description: string
  title: string
}

@Injectable({
  providedIn: 'root',
})
export class FirestoreService {
  private boardCollection: AngularFirestoreCollection<Board>

  constructor(private afs: AngularFirestore) {
    this.boardCollection = afs.collection<Board>('books')
  }

  getBoards(): Observable<Board[]> {
    return this.boardCollection.snapshotChanges().pipe(
      map(actions =>
        actions.map(a => {
          const data = a.payload.doc.data()
          const id = a.payload.doc.id
          return { id, ...data }
        }),
      ),
    )
  }

  getBoard(id: string): Observable<any> {
    return this.boardCollection.doc(id).valueChanges()
  }

  postBoard(board: Board) {
    return from(this.boardCollection.add(board).then(data => data.id))
  }

  updateBoards(id: string, board: Board) {
    const doc = this.boardCollection.doc(id)
    return from(doc.update(board))
  }

  deleteBoards(id: string) {
    const doc = this.boardCollection.doc(id)
    return from(doc.delete())
  }
}
```

The above code uses [AngularFireStore Collection API](https://github.com/angular/angularfire2/blob/master/docs/firestore/collections.md) to get the list of all boards. It uses the [`snapshotChanges()`](https://github.com/angular/angularfire2/blob/master/docs/firestore/collections.md#snapshotchanges) method to get the document Id that will be used in HTML. Other methods uses [AngularFireStore Document API](https://github.com/angular/angularfire2/blob/master/docs/firestore/documents.md) to implement the creation, update and deletion opertaions.

## 7 The CRUD Operations

### 7.1 The List of Boards

The list of boards page uses the [`<table mat-table>`](https://material.angular.io/components/table/overview) to display a data table. The data table element requires a `[dataSource]` attribute that could be an array or an oberservable of array. Using an observable lets the data table observe the data changes. Each table row also defines a `[routerLink]` attribute that works as a link to the detail page. The links requires the document id to point to the specific document.

### 7.2 The Detail/Delete Page

The detail page gets the documnet id from it route parameter using `this.route.snapshot.params['id']`. Then it calls the Firebase service to get the document details.

The page has a delete icon that when clicked, calls the Firebase service `delete` method to delete the document and navigates to the list page.

The edit icon links to the edit page.

### 7.3 The Edit Page

The edit page uses [Angular reactive form](https://angular.io/guide/reactive-forms) to edit the board fields. When the submit button is clicked, it updates the board and navigates to the list page.

### 7.4 The Create Page

Similar to the edit page, the create page uses reactive form to create a board document and saves it to the Firebase backend. All fields are required. If a field is empty, it displays an error message.
