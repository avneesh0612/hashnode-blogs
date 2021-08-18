## Create a Custom Sign-in Page with NextAuth.js


How to create a custom sign-in page with NextAuth.js

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627311915653/u8tSgMsi4.png)

Today, we are going to see how to build a custom login page with NextAuth.js.

In one of my previous articles, we saw what is NextAuth and how to build Google authentication with it. If you haven’t read it already, you can check it out [here](https://medium.com/geekculture/why-and-how-to-get-started-with-next-auth-61740558b45b).

### Let’s get started

In ***[…nextauth].js***, we will add a pages property below providers like this:

```
pages: {
signIn: "/signin",
},
```


Now, our** *[…nextauth].js*** should look something like this


### Creating the sign-in page

In the pages directory, we are going to create a new page ***signin.js***.

We are going to need a bunch of imports from the next-auth/client package. Here are all the imports:

```
import { providers, signIn, getSession, csrfToken } from "next-auth/client";
```


We will now create a simple React functional component:


### Showing all the providers

Inside our component, we are going to map out all the providers in this case we have only Google sign-in.

We first need to get all the providers, we are going to use getServerSideProps to get all the providers:


This is going to give us all the providers we have added in ***[…nextauth].js*** and we are going to receive the providers as a prop.


This is going to give us a simple button saying **‘Sign in with Google’**. If we click on this button our sign-in flow should still work properly. Now if we sign in, we will be redirected to the sign-in page and not the home page.


We will now see how to fix this.

In the `getServerSideProps` function we are going to add this:


Now if we sign in, we will be redirected to the home page, if you want to redirect the user to some other page change the location in the destination.


The final *signin.js* looks like this


### Styling the page

Now I am going to apply some simple stylings to the button and center it. I am going to use CSS modules for the styles as it is built-in, you can use anything you like.

* Create a file ***Signin.module.css***

I will add a className to the container div and the button like this


We will import styles from ***Signin.module.css*** like this

```
import styles from "../styles/Signin.module.css";
```


* Finally, we will add the styles in ***Signin.module.css*** to make it look good


After applying these styles we get a centered, good-looking button.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627311917312/NK77m217Y.png)

I hope you could create your first custom sign-in page with Next auth.

**Useful links:**

[Github repository](https://github.com/avneesh0612/next-auth-custom-login)

[NextJS docs](https://nextjs.org/docs)

[Next auth](https://next-auth.js.org/)

[All socials](https://avneesh-links.vercel.app/)

*More content at [**plainenglish.io](http://plainenglish.io)***