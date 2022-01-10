# Firebase 9 Course By NetNinja

These are the notes from the [firebase 9 course](https://netninja.dev/p/getting-started-with-firebase-9) I took from netninja.



## Initialize Firebase

```js
import { initializeApp } from "firebase/app";

const config = { /* your config */ }

initializeApp(config);
```

## How to get docs in a collection

```js
import {
  getFirestore,
  collection,
  getDocs,
} from "firebase/firestore";

const db = getFirestore();
const colRef = collection(db, "books");
getDocs(colRef).then((snapshot) => {
    const docs = snapshot.docs;
    docs.map(d => {
    	return {
	   ...d.data(), // get json data
	   id: d.id
	}
    })
});
```

## How to get a single doc


```js
import {
  getFirestore,
  getDoc,
  doc
} from "firebase/firestore";

const db = getFirestore();
const docRef = doc(db, "books", "firebase_id");
getDoc(docRef).then((doc) => {
  console.log(doc.data(), doc.id, "single doc");
});
```

## How to create a doc

```js

import {
  getFirestore,
  collection,
  addDoc,
  serverTimestamp
} from "firebase/firestore";


const db = getFirestore();
const colRef = collection(db, "books");


addDoc(colRef, {
    title: "example title",
    author: "example author",
    createdAt: serverTimestamp(),
  }).then(() => {
	// Do something
   });
```

## How to update a doc

```js
import {
  getFirestore,
  doc,
  updateDoc
} from "firebase/firestore";

  const db = getFirestore();

  const docRef = doc(db, "books", "firebase_id");

  updateDoc(docRef, {
    title: "updated title",
  }).then(() => {
    
  });
```

## How to query a doc

```js
import {
  getFirestore,
  collection,
  query,
  where,
  orderBy,
  getDocs
} from "firebase/firestore";

const db = getFirestore();
const colRef = collection(db, "books");

const q = query(
  colRef,
  where("author", "==", "Dave Berry"),
  orderBy("createdAt")
);

// get collection data
getDocs(q)
  .then((snapshot) => {
    let books = [];
    snapshot.docs.forEach((doc) => {
      books.push({
        ...doc.data(),
        id: doc.id,
      });
    });
    console.log(books, "ordered");
  })
  .catch((err) => console.log(err.message));

```

## How to login

```js
import {
  getAuth,
  createUserWithEmailAndPassword
} from "firebase/auth";

  const auth = getAuth();

  signInWithEmailAndPassword(auth, email, password)
    .then((cred) => {
      //   console.log("user logged in: ", cred.user);
      loginForm.reset();
    })
    .catch((err) => console.log(err.message));

```

## How to logout 

```js

import {
  getAuth,
  signOut
} from "firebase/auth";

const auth = getAuth();

signOut(auth)
    .then(() => console.log("user signed out"))
    .catch((err) => console.log(err.message))
```

## How signup a user

```js

import {
  getAuth,
  createUserWithEmailAndPassword
} from "firebase/auth";

const auth = getAuth();

  createUserWithEmailAndPassword(auth, "e@gmail.com", "password")
    .then((cred) => {
      //   console.log("user created: ", cred.user);
    })
    .catch((err) => console.log(err.message));
```


## How to listen for auth events

```js
import {
  getAuth,
  onAuthStateChanged
} from "firebase/auth";

const auth = getAuth();

onAuthStateChanged(auth, (user) => {
  console.log("user status changed:", user);
});
```

## How to unsub from listeners

```js
const unsubAuth = onAuthStateChanged(auth, (user) => {
  console.log("user status changed:", user);
});

// then when you want to unsub 

unsubAuth();
```