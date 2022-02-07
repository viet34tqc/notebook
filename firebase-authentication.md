# Firebase Authentication

## Step 1: Create a Firebase project and Firebase App

## With Google

### Step 2: Setup Authentication

- Go to <i>Authentication -> Get Started -> Enable Google</i>
<img src="https://i.imgur.com/5CYQLlp.png">

### Step 3: Add Firebase code to your project

- Create a `firebase.ts` in your `root` folder
- Add these code to `firebase.ts`
```typescript
import firebase from 'firebase';
var firebaseConfig = {
	apiKey: 'AIzaSyDkcFEsxsVzZDLGxymImLcWBFl1ZUvgPWE',
	authDomain: 'disneyclone-3aae5.firebaseapp.com',
	projectId: 'disneyclone-3aae5',
	storageBucket: 'disneyclone-3aae5.appspot.com',
	messagingSenderId: '1024777757861',
	appId: '1:1024777757861:web:fb583f520a41b045b9594c',
	measurementId: 'G-6KV11WQY91',
};
// Initialize Firebase
const firebaseApp = firebase.initializeApp(firebaseConfig);

// Authentication
const auth = firebase.auth();
const provider = new firebase.auth.GoogleAuthProvider();

// Firebase Store
const db = firebaseApp.firestore();

const storage = firebase.storage();

export { auth, provider, storage };
export default db;
```
- Create a login button

### With email/password

```js
import { getAuth } from 'firebase/auth';
export const auth = getAuth(); // No need to create firestore database

// Register
const user = await createUserWithEmailAndPassword(
	auth,
	registerEmail,
	registerPass
);

// Logout
await signOut(auth)
```

