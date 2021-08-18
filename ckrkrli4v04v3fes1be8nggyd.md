## Why and how to get started with Next auth?


Google authentication with Next.js

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312063095/tSzOSAz40.png)

### Why should you choose Next auth?

You might be wondering, if you are already using something like firebase then why should you use Next auth? Here’s why

* Easy to use

* Gives us a lot of providers for authentication

* You can get the details as soon as the user reaches the page with serverSideProps while you would get the user's details after a few seconds with something like firebase.

* It is very secure as it uses HTTP POST + CSRF Token validation
JWT with JWS / JWE / JWK / JWK

### Setup

I am going to set up a new next app with create-next-app.

```
npx create-next-app next-auth-demo
```


Cleanup

I will delete everything after the Head starting from the main till the footer.

Install next-auth

```
npm i next-auth # npm
yarn add next-auth # yarn
```


### Start using Next auth

We will first create a folder inside the api folder in pages called auth.

In that folder create a file called […nextauth].js

We are going to start with Google authentication. So add this snippet of code in your […nextauth].js.


Now we will finally configure Next auth in _app.js like this -


### Google Authentication

We are going to use firebase to create the credentials for us. We can use Google cloud console directly but we will need to configure extra stuff.

Go to [firebase](https://console.firebase.google.com/u/0/). Create a new account then click on add project.

Give a name for your app then you can leave everything as default and click next.

Go to the authentication tab in the sidebar. Click get started. Click on Google sign in and enable it. Now simply click on save.

If you click on edit again and open the Web SDK configuration accordion. You can see the credentials we need. I am showing my credentials for the demo but you shouldn’t show your credentials.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312065483/UHNmv_ZJ6.png)

Now we will create a new file called .env.local in the root of our folder. In this file, we will create two environment variables called GOOGLE_CLIENT_ID and GOOGLE_CLIENT_SECRET.

Now copy-paste the client id and secret here.


We will create another variable called NEXTAUTH_URL which will be our app URL which in development will be our localhost.


After adding the variables make sure to restart the server.

We will create a Sign-in button in index.js


Now if we click on the Sign-in button it will show an error similar to this


Now copy the link at which is at the bottom of the error and paste the link into a new tab. Scroll down and you will be able to see redirect URI’s.

![Redirect URIs](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312067658/TYoqR06A0.png)*Redirect URIs*

Now click on ADD URI and add this

```
 [http://localhost:3000/api/auth/callback/google](http://localhost:3000/api/auth/callback/google)
```


Then click on save. If you try logging in. You can now log in.


### Getting user info

We will first fetch the data on the client then we will see how to fetch it with serverSideProps.

For fetching the user’s info on the client we will use the useSession hook.

The useSession hook gives us an object of session which has the user’s info inside it.


Now let’s see what we get back on signing in


We would like to have a signout button if the user is already signed in. So we will have a sign-in button if there is no user and a signout button if there is a user.

So create a ternary operator like this


And then import signOut from next-auth/client.


### Fetching the user data on the server

We will create a function with serverSideProps where we will get our user details via session.


Instead of importing useSession, we will import getSession from next-auth/client

```
import { getSession, signIn, signOut } from "next-auth/client";
```


It will now fetch the data instantly without the delay.

### Deployment

I will create a new GitHub repository and push the code there. If you don’t about git and GitHub. Check my [Git and Github Crash Course](https://avneeshagarwal.medium.com/git-and-github-crash-course-b44f4885ff66).

After that go to [Vercel](https://vercel.com/)

* Sign up with your GitHub

* Click on create a new project

* Import the repository that you created

* Click deploy

Your site will now be deployed but the authentication won’t work.

Adding environment variables.

* After the site is deployed go to the dashboard of the current app click on settings and then Environment Variables.

* Add the Google client id and Google client secret which is present in our .env.local file.

* After you add the two it will look like this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312069305/3OSFFR8DH.png)

* We will add another variable which is our next auth URL and it will be the deployed URL instead of the localhost.

* To get the URL click on domains and get the shortest link and add it to the NEXTAUTH_URL variable. It would be similar to [https://next-auth-demo-lemon.vercel.app/](https://next-auth-demo-lemon.vercel.app/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312070968/ThJNocb_k.png)

* Finally, go to the Deployments tab on the tab in the header.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312072791/3gJT2HnuI.png)

* Click on the latest version and press redeploy.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312074471/U_1H8pst5.png)

### Adding the URI to Google

After you redeploy try signing in again and it will show you an error.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312076364/qHCen8MTF.png)

* Go to the URL at the bottom of the error.

* Create a new URI and paste the link after ‘The redirect URI in the request’ which is [https://next-auth-demo-lemon.vercel.app/api/auth/callback/google](https://next-auth-demo-lemon.vercel.app/api/auth/callback/google) in my case.

Our authentication is now working perfectly

Congratulations 🥳. You have created your first Google Sign-in with Next auth. If you wanna customize the login page click [here](https://javascript.plainenglish.io/how-to-create-a-custom-sign-in-page-in-next-auth-1612dc17beb7). If you wanna add Auth0 authentication click [here](https://javascript.plainenglish.io/how-to-authenticate-users-with-auth0-in-next-auth-9c1160ce48a8).

Let me know if you want to see any other kind of authentication😉.

Useful links -

[Github repository](https://github.com/avneesh0612/next-auth-demo)

[NextJS docs](https://nextjs.org/docs)

[Next auth](https://next-auth.js.org/)

[All socials](https://avneesh-links.vercel.app/)