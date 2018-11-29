# Firebase Application

This chapter describes the CRUD application based on Firebase SDK.

## A CRUD Service

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
