# Firebase Authentication and Firestore

Firebase provides an easy way to authenticate user. However, when the user is authenticated, we also need to save that user into db, which is firestore

1. User authenticate
2. Save user into firestore
3. Then if we want to check that logged in user, fetch user profile documentation from DB (Firestore)

## Firestore concept

- Each project can has multiple `collection`. 
- Each collection has `document`, `document` is like a row in database. A `document` can include another `collection`, as `subcollection` (ex: in a user document can have a collection of posts)
- A reference is a lightweight object that just points to a location in your database, which could be a `collection` or a `document`.
- Collection references and document references are two distinct types of references and let you perform different operations. For example, you could use a collection reference for querying the documents in the collection, and you could use a document reference to read or write an individual document.

## Create a Firebase project and Firebase App


- Go to <i>Authentication -> Get Started -> Enable Google</i>
<img src="https://i.imgur.com/5CYQLlp.png">

- Add Firebase code to your project
  - Create a `firebase.ts` in your `root` folder
  - Add these code to `firebase.ts`
```js
import firebase from 'firebase';
var firebaseConfig = {
	apiKey: 'AIzaSyDkcFEsxsVzZDLGxymImLcWBFl1ZUvgPWE',
	authDomain: 'disneyclone-3aae5.firebaseapp.com',
	projectId: 'disneyclone-3aae5',
	storageBucket: 'disneyclone-3aae5.appspot.com',
	messagingSenderId: '1024777757861',
	appId: '1:1024777757861:web:fb583f520a41b045b9594c',
};
// Initialize Firebase
const app = initializeApp(firebaseConfig); // v8: firebase.initializeApp(firebaseConfig);

// Authentication
const auth = getAuth(app) // v8: firebase.auth();
const googleAuthProvider = new GoogleAuthProvider() // v8: new firebase.auth.GoogleAuthProvider();

// Firebase Store
const db = getFirestore(app); // v8: firebaseApp.firestore();

export { auth, googleAuthProvider, db };
```

## Login with Google and save user to firestore

```js
// With google
const signInWithGoogle = async () => {
	try {
		const user = await signInWithPopup(auth, googleAuthProvider);
		// We use `setDoc` here to set custom id for user
		// If the data doesn't have specific id, use `addDoc` to automatically add íd to document
		await setDoc(doc(firestore, 'users', user.user.uid), {
			displayName: user.user.displayName,
		});
	} catch (error) {
		console.log(error);
	}
};
```

## Login with email/password

```js
import { getAuth } from 'firebase/auth';
export const auth = getAuth(); // No need to create firestore database

await signInWithEmailAndPassword(auth, email, password);
```


## Register With email/password

```js
import { getAuth } from 'firebase/auth';
export const auth = getAuth(); // No need to create firestore database

// Register
const user = await createUserWithEmailAndPassword(
	auth,
	email,
	password
);
```

## Logout

```js
await signOut(auth)
```

## Read data

- Create the reference. This is like query statement
  - Document reference: `doc(db, NAME_OF_COLLECTION, ID_OF_DOCUMENT )` 
  - Collection reference: `collection(db, NAME_OF_COLLECTION)`

```js
const docRef = doc(firestore, 'users', user.uid);
```

- Get doc from reference: `getDoc`

```js
await getDoc(docRef);
```

- Get doc from query

```js
const q = query(docRef, where('displayName', '==', username), limit(1));
const userDoc = (await getDocs(q)).docs[0];
```

- Get data back from reference

```js
docRef.data()
```

## Write data

- Add: `setDoc` (custom document ID) and `addDoc` (automatically generated ID)

```js
await setDoc(docRef, {
	uid: user.user.uid,
});
```

`addDoc` uses `collection` instead of `doc`

```js
await addDoc(collection(db, 'usernames'), {
	uid: user.user.uid,
});
```