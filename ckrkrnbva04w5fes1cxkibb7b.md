## How to build Google Sign in with firebase in React with react-firebase-hooks


Did you know that Google provides a suite of tools in its Firebase platform? With Firebase, you can store data in its NoSQL database, build user authentication and many more. Let’s learn how to build user authentication in a React app with Firebase!

![Firebase and react user authentication](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312153845/TZg-ja1B2.png)*Firebase and react user authentication*

### Creating a new react app

Prequiesties for creating a react app-

* [Node.js](https://nodejs.org) (Latest version recommended)

After node is installed you need to run the following command-

```
npx create-react-app firebase-auth-demo # you can name it anything you want
```


### Starting the app -

```
# npm
npm start
# yarn
yarn start
```


### Installing the required dependencies -

```
# npm
npm install react-firebase-hooks firebase
# yarn
yarn add react-firebase-hooks firebase
```


### Cleanup process

* Delete everything inside the div in App.js and remove the import of logo.svg on top.

* Delete App.test.js, SetupTests.js, logo.svg files.

* Delete everything in App.css.

* In index.css add this line of code -

```
* {
  margin: 0;
}
```


### Setting up firebase

Go to [Firebase](https://firebase.google.com/) and create an account if you don’t have an existing one. Then set up a new project and you can name it anything you like. After creating a new project you will see a code icon and click on it to create a web app and name it the same as your project.

![Code icon](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312155679/HpsT8EqQF.png)*Code icon*

After you have created the web app go to the project settings tab and scroll to the bottom and select config and copy it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312157300/UsHQxGXB5.png)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312159671/t483X0o7Z.png)

Then you need to go to the authentication tab. Click get started and enable Google Sign in and click Save.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312161475/NoTli2ace.png)

### Connecting firebase to our app

Now go to the src folder in your react app and create a new file called firebase.js and paste the config. You then need to import firebase from firebase module like this -
```
import firebase from "firebase";
```


Then you need to initialize the app and create and export auth and provider (which is Google in our case) variables -
```
const firebaseApp = firebase.initializeApp(firebaseConfig);
const auth = firebaseApp.auth();
const provider = new firebase.auth.GoogleAuthProvider();

export { auth, provider };
```

Your firebase.js file should look like this and you need to use your config instead-

```
import firebase from "firebase";

const firebaseConfig = {
  apiKey: "AIzaSyDcm355Vya1Kegi_AIBXENL2UgjZwE3vYU",
  authDomain: "fir-auth-demo-99bf4.firebaseapp.com",
  projectId: "fir-auth-demo-99bf4",
  storageBucket: "fir-auth-demo-99bf4.appspot.com",
  messagingSenderId: "825591458421",
  appId: "1:825591458421:web:dd566d263ac9fad0f1663c",
  measurementId: "G-FD6JRHLCX9",
};

const firebaseApp = firebase.initializeApp(firebaseConfig);
const auth = firebaseApp.auth();
const provider = new firebase.auth.GoogleAuthProvider();

export { auth, provider };
```


### Creating a login page

Create a button with an onClick function of signIn -
```
<button onClick={signIn}>Sign In</button>
```


The signIn function will be like this -
```
const signIn = () => {
    auth.signInWithPopup(provider).catch((error) => alert(error.message));
  };
```


Finally importing auth and provider from our local firebase file-
```
import { auth, provider } from "./firebase";
```

### Rendering the login page -

In app.js you need to render the login page so inside the div you need to add the Login component and import it.

```
import "./App.css";
import Login from "./Login";

function App() {
  return <Login />;
}

export default App;
```

Your sign-in button should be working now. 🎉🥳

### Styling the login page -

We will style the button just a little bit. So just follow the next steps to align the button centrally and change its color.

* Create a file called Login.css and import it at the top as-
```
import "./Login.css";
```


* Create a div enclosing the button inside it and give it a className of login like this -
```
  <div className="login">
      <button onClick={signIn}>Sign In</button>
    </div>
```


* Then just adding this simple styling -

```
.login {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 50vh;
}

.login > button {
  width: 400px;
  height: 100px;
  color: white;
  font-size: 30px;
  background-color: #ffbd2e;
}
```

### Doing something when the user signs in

In App.js you need to get the user from the *useAuthState *hook like this-
```
const [user] = useAuthState(auth);
```


and import useAuthState and auth like this -
```
import { useAuthState } from "react-firebase-hooks/auth";
import { auth } from "./firebase";
```


Then in the return function, I am adding this -
```
 <>
      {user ? (
        <div className="app">
          <h1>Hello, {user.displayName}</h1>
          <h1>You are signed in as {user.email}</h1>
          <button onClick={signOut}>Sign Out</button>
        </div>
      ) : (
        <Login />
      )}
    </>
```


This is called ternary operator which means that if there is a user then render out the div with className of app which includes two h1 tags which shows the name and email and a signout button with which the user can sign out. In the next section, I am telling about the signOut functionality. And if there is no user then render out the login page.

### Signing out of the app -

To create a sign out button create a button with an onClick function of signout
```
<button onClick={signOut}>Sign Out</button>
```


The onClick function would be this -
```
 const signOut = () => {
    auth.signOut();
  };
```

e.preventDefault prevents the page from reloading. And authentication with Firebase is so easy that they have this function called .signOut to sign out the user.

Finally, import auth -
```
import { auth } from "./firebase";
```


You have successfully created your first Google sign-in with React and Firebase🥳.

Thank You!
