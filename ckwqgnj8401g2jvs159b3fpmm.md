## Metamask authentication with Moralis in Next.js

If you haven't been living under a rock you have probably heard of Web 3.0!
![Living under a rock](https://media4.giphy.com/media/WRQiZ1rkueNh0Dw1mQ/giphy.gif)

One of the most important parts of a full-stack is authentication. So let's see how to authorize users with their Metamask wallet in a Next.js app.

If you don't know what is metamask then check out their  [website](https://metamask.io/) 

## Setting up the app
**Create a new next app**

```
npx create-next-app next-metamask
```

**Navigate into the app**

```
cd next-metamask
```

**Installing the required dependencies**

```
npm i @walletconnect/web3-provider moralis react-moralis # npm

yarn add @walletconnect/web3-provider moralis react-moralis # yarn
```

**Start the server**

```
npm run dev # npm

yarn dev # yarn
```


### Get Moralis Credentials

Go to [moralis](https://moralis.io/) and signup/login. After that click on Create new Server and select `TestNet Server`

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1638519444572/c63mPpZZ6.png)

By selecting it you will see a popup. Fill in the details, and click the `Add Instance` button.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1638519513809/x41ig0cC7.png)


After the server is created click on `view details `


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1638521693161/4xVCAOJGCO.png)


We are going to need the server URL and Application ID 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1638521770935/AG8Yv6dXz.png)


## Building out the authentication system
**Adding the environment variables**

Create a `.env.local` file in the root of your folder and add the env variables like this-

```
NEXT_PUBLIC_MORALIS_APP_ID=<app_id>
NEXT_PUBLIC_MORALIS_SERVER_ID=<server_id>
```

You need to replace the values of the variables with the credentials you got from Moralis.


**Wrap the app in a MoralisProvider**

Go to `_app.js` and wrap the `<Component {...pageProps} />` with the Moralis Provider with the env variables like this-

```
<MoralisProvider
  appId={process.env.NEXT_PUBLIC_MORALIS_APP_ID}
  serverUrl={process.env.NEXT_PUBLIC_MORALIS_SERVER_ID}
>
  <Component {...pageProps} />
</MoralisProvider>
```

Now import `MoralisProvider` from react-moralis

```
import { MoralisProvider } from "react-moralis";
```

**Creating the sign-in button**
I am going to do create the login button on the home page, feel free to create it on any page you need.

Get the authenticate function from the useMoralis hook-

```
const { authenticate } = useMoralis();
```

You also need to import the hook from react-moralis

```
import { useMoralis } from "react-moralis";
```

Create a button like this-

```
<button
  onClick={() => {
    authenticate({ provider: "metamask" });
  }}
>
  Sign in with MetaMask
</button>
```

Now if we click on sign in, it will open the metamask extension for sign in.

%[https://www.loom.com/share/c2b1c9936ad44185810d9978ea5c2193]

**Show signout button if the user is signed out**

We need to get a few more things from the `useMoralis` hook like this-

```
const { authenticate, isAuthenticated, logout } = useMoralis();
```

Create a ternary operator to show the logout button, if the user is signed in otherwise show the sign-in button-

```
{isAuthenticated ? (
    <button
      onClick={logout}
    >
      Logout
    </button>
  ) : (
    <button
      onClick={() => {
        authenticate({ provider: "metamask" });
      }}
    >
      Sign in with MetaMask
    </button>
  );
}
```


Now our sign in and sign out is working 🥳🥳

**Getting the user data**
Let's see how to get some basic data like their eth address and username.

When the user is authenticated, you can add this fragment to show the userName and their address wallet-

```
<>
  <button onClick={logout}>Logout</button>
  <h2>Welcome {user.get("username")}</h2>
  <h2>Your wallet address is {user.get("ethAddress")}</h2>
</>
```

You need to get the user from the `useMoralis` hook as well-

```
const { authenticate, isAuthenticated, logout, user } = useMoralis();
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1638538592544/Mmo4idnMF.png)

The userName is very random 😂but it can help in some cases and the eth address can be used for transactions.

### Signing off

It was this easy to implement authentication of metamask with moralis 🤯

Hope you found this tutorial useful and stay tuned for more of these web 3.0 tutorials ✌️


### Useful links

 [GitHub repo](Link) 

[Moralis](https://moralis.io/) 

[Metamask](https://metamask.io/) 

[Connect with me](https://links.avneesh.tech)
