## How to start a headless E2E e-commerce store in no time with Medusa 🛍️

Wassup everyone, in this article we are going to create a headless e-commerce application using an open-source alternative to Shopify-  [**Medusa**](https://www.medusajs.com/).

![](https://c.tenor.com/6Hixx4SFAeQAAAAM/backing-you-get-yours.gif)

## What are we building?

%[https://www.loom.com/share/0b3dd851cb2d449f8c452369aa514017]

## What is Medusa?

Medusa is an open-source headless commerce platform which makes it super easy to build an e-commerce site quickly! It is an amazing tool so let's see why to choose Medusa-

- Easy to use
- Scalable
- Open-source
- Easily customizable
- Free to get started with!
- Large supporting community to help
- Many, easy to use integrations


## Prerequisites

We are going to need some stuff installed on our machine so let's see what those are

### Node.js

Our backend server is built with Node.js so we are gonna need it to run the server. Our frontend is also gonna be built with Next.js which requires npm so we are gonna need it for that too! So, go to  [the Node.js website](https://nodejs.org/en/) and install recommended version.

### PostgreSQL

For storing data of your e-commerce website Medusa uses Postgres. You can go to the [site](https://www.postgresql.org/download/) to install Postgres for your system!

### Redis

Redis is another database commonly used for caching. You can download it from [here](https://redis.io/).


### Medusa CLI

Finally, we are going to need the Medusa CLI. Since we already have Node.js installed on our machine, we can easily install it from npm using this command-

```
npm i -g @medusajs/medusa-cli
``` 

## Quickstep to step guide for beginners

### Creating our backend server

We are going to need a backend for your e-commerce store so in this part let's see how to build it! 

**Create a starter server using Medusa**

The Medusa CLI makes it super easy to create a new server. So, run this command-

```
medusa new my-medusa-server --seed
```

This is going to create a basic server for us to start with, a Postgres database, and the --seed flag is also going to add some test data to the database.


**Starting the server**

Navigate into the app-

```
cd my-medusa-server
```

Start the server, using Medusa-

```
medusa develop 
```

This will start a server on  [http://localhost:9000](http://localhost:9000) 

### Creating frontend for the store

I am going to use Next.js for the front end of our store, Medusa also supports Gatsby and Create React app. I am going to use a starter template. So run this command-

```
npx create-next-app -e https://github.com/medusajs/nextjs-starter-medusa my-medusa-storefront
```

This will create a new next js project for us. If you want to use tailwindCSS, you can try out this  [template ](https://github.com/avneesh0612/next-medusa-tailwind-template) that I created.

Rename `.env.template` to `.env.local`. You can also do it with one command-

```
mv .env.template .env.local
```

Run the app-

```
npm run dev
```

If you now go to [localhost:8000](http://localhost:8000), you will be able to see a simple frontend for our store. 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641371654370/2hml3NYdm.png)

This can be customized too! So, open it in your favorite code editor I am going to use VScode.

If you have worked with next.js before it is going to look like a normal Next.js app to you! Let's start by changing the logo of the app. Go to `components/layout/nav-bar.jsx` and change the src of the Image to your logo. For now, I am going to use a simple house image from icons8. You might need to change the width and height based on the dimensions of your image-

```
<a>
  <Image
    src="https://img.icons8.com/fluency/96/000000/shop.png"
    height="50px"
    width="50px"
    alt="logo"
  />
</a>
```

Now, the logo has been changed!

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641390215492/0yvrHmvMN.png)

**Customizing the colors**

Go to `globals.css` and inside `:root` you will see a bunch of colors, replace those with your brand colors. This will change the overall theme of the site, if you want to change more CSS then you can just go to that component and customize it 😉. After some tweaks and changes, you will have a completely custom functional store!

By doing only this much, our app is up and running!

%[https://www.loom.com/share/0b3dd851cb2d449f8c452369aa514017]


## Contributing to Medusa as a beginner

If you are fascinated by open-source projects and contributing to amazing projects then you should try to contribute to Medusa. The first question would be **how**? If you find an issue in a package, or the site then creates a *Pull Request* fixing it. If you don't know how to create a pull request check out this  [article ](https://blog.avneesh.tech/how-to-contribute-to-an-open-source-project) and it is super easy! Medusa has many repositories under its  [organization](https://github.com/medusajs/). The most popular one is the  [main repository](https://github.com/medusajs/medusa). You can contribute to it through documentation, issues, and many more!


## Conclusion
I hope you could set up your e-commerce site using medusa easily. If you have any questions let me know in the comments 👇😉.


### Useful links

[GitHub Repo](https://github.com/avneesh0612/medusa-store) 

[Medusa](https://www.medusajs.com/) 

[Join the Medusa discord server](https://discord.gg/NZZ4ymyESg)

[Connect with me](https://links.avneesh.tech/)
 