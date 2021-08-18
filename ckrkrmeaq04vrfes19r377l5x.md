## Authenticate Users with Auth0 in Next.js


How to Authenticate Users with Auth0 in Next

![Next auth + Auth0](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312100735/7riao5eKy.png)*Next auth + Auth0*

### What is Auth0?

Auth0 is an easy-to-implement, adaptable authentication and authorization platform. If you want to follow along with this tutorial, you can create a free Auth0 account [here](https://auth0.com/signup?utm_source=external-post&utm_medium=link_placement&utm_campaign=devmar-media).

### Setup

I am going to set up a new next app with create-next-app.

```
npx create-next-app next-auth0-demo
```


### Cleanup

I will delete everything after the Head starting from the main till the footer.


Install [Next-Auth](https://next-auth.js.org/)

```
npm i next-auth # npm
yarn add next-auth # yarn
```


### Start using Next auth

We will first create a folder inside the `api` folder in pages called `auth`.

In that folder create a file called ***[…nextauth].js***

We are going to use auth0 as the provider, so we need three params. So add this snippet of code in your ***[…nextauth].js***.


Now we will finally configure Next Auth in ***_app.js*** like this -


### Setting up an application in Auth0

**Creating an app**

* Sign up to [Auth0](https://auth0.com/signup?utm_source=external-post&utm_medium=link_placement&utm_campaign=devmar-media)

* Go to applications under the applications tab

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312102813/aeumWusar.png)

* Create a new application

* Select Regular Web Applications and give a name to your app, then click on create.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312104666/Gf5JnkpMT.png)

**Getting the credentials**

In the settings tab copy the domain,`client id` , and the `client secret`and add it to your .env.local file.


We will also add the NEXTAUTH_URL in the env like this

```
NEXTAUTH_URL=http://localhost:3000
```


### Creating a sign-in button


Now if we try to sign in, we will see this error page

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312106571/07_AgYu6b.png)

We need to add [http://localhost:3000/api/auth/callback/auth0](http://localhost:3000/api/auth/callback/auth0) as an allowed callback URL and save it.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312108605/rlzwYHP8c.png)

Now we can sign in successfully!


### Getting user info

We will create a function with `getServerSideProps`where we will get our user details via session.


We would like to have a signout button if the user is already signed in. So we will have a sign-in button if there is no user and a signout button if there is a user.

So create a ternary operator like this


And then import everything we need from next-auth/client.


### Showing the user details

We are going to add this code to get the user’s email, name, and image.


The final code should be like this


As we are using the Next image, we will need to add the domain in ***next.config.js***

We are using google sign in, so we will add lh3.googleusercontent.com as the domain, if you don’t know the origin of the image you can use the normal img tag instead.


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

* Add the Auth0 client id, Auth0 client secret, and Auth0 domain which is present in our .env.local file.

* After you add the two it will look like this.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312110389/zYxAcSOm1.png)

* We will add another variable, our Next Auth URL and it will be the deployed URL instead of the localhost.

* To get the URL, click on domains, get the shortest link, and add it to the NEXTAUTH_URL variable. It would be similar to [https://next-auth0-demo.vercel.app/](https://next-auth0-demo.vercel.app/)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312112188/xjX8PRdqY.png)

* Finally, go to the Deployments tab on the tab in the header.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312113893/UpAs8m9GX.png)

* Click on the latest version and press redeploy.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312115617/fJFDT4sK5.png)

### Change the URI in Auth0

Finally, replace localhost:3000 with the actual URL and use HTTPS instead of HTTP.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627312117774/wbsSxqkNP.png)

Our authentication is now working perfectly. Congratulations. You have created your first Auth0 sign-in with Next Auth! 🥳

If you have found this useful, be sure to let me know in the comments. You can also let me know if you want to see any other kind of authentication.

### Further resources

* [Github repository](https://github.com/avneesh0612/next-auth0-demo)

* [NextJS docs](https://nextjs.org/docs)

* [Next Auth](https://next-auth.js.org/)

* [Auth0](https://auth0.com/)

* [All socials](https://avneesh-links.vercel.app/)

*More content at [**plainenglish.io](http://plainenglish.io)***