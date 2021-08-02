## Stop using Linktree! Build your own 😎

### What are we building?
%[https://www.loom.com/share/2535312c245247e6ab4348e9400b0009]

### Setting up

#### Creating a Next app with TailwindCSS

```
npx create-next-app -e with-tailwindcss links-app
```

#### Cleanup

Delete everything in ***pages/index.js*** after the Head till the footer it should look like this
 
```
import Head from 'next/head'

export default function Home() {
  return (
    <div className="flex flex-col items-center justify-center min-h-screen py-2">
      <Head>
        <title>Create Next App</title>
        <link rel="icon" href="/favicon.ico" />
      </Head>

    </div>
  )
}
```

#### Starting the app

```
npm run dev # npm
yarn dev # yarn
```

### Creating a ```Links``` component

We will create a ```components``` folder in the root of the directory and create a file ***Links.js***.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627898330167/3-P_1Qpbo.png)

#### Creating a functional component
I will create a functional component like this in the links file.
```
function Links() {
    return (
        <div>
            
        </div>
    )
}

export default Links
```

We need to add 3 props ```image```, ```text``` and ```link``` because we are going to reuse this component instead of pasting the same code again and again.

```
function Links({ image, text, link }) {
```

We will now use the props and render out an anchor tag with an image and text

```
import Image from "next/image";

function Links({ image, text, link }) {
  return (
    <a href={link}>
      <Image width={40} height={40} src={image} alt={text} />
      <h2>{text}</h2>
    </a>
  );
}

export default Links;
```

### Rendering the Links

Now in ***index.js*** we will add the Links component with the props below the Head

``` 
<Links
        image="https://cdn.hashnode.com/res/hashnode/image/upload/v1611902473383/CDyAuTy75.png?auto=compress"
        link="https://avneesh0612.hashnode.dev/"
        text="Checkout my blogs on Hashnode"
      />```

After we add this we will see an error as we are using the optimized Next image. 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627898744752/-i_8dbc4T.png)

We need to whitelist the domain in next.config.js.

- So create a file ***next.config.js*** in the root of the project and add the following.
```
module.exports = {
  images: {
    domains: ["cdn.hashnode.com"],
    },
};
```
- Now we need to restart the server and the error is not there anymore.
- After following the steps you will be able to see this

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627899246688/Gw-YEsIlt.png)

### Adding more links
I am adding these links 
```
<Links
          image="https://upload.wikimedia.org/wikipedia/commons/e/e7/Instagram_logo_2016.svg"
          link="https://www.instagram.com/avneesh.codes/"
          text="Follow me on Instagram"
        />
     <Links
        image="https://cdn.hashnode.com/res/hashnode/image/upload/v1611902473383/CDyAuTy75.png?auto=compress"
        link="https://avneesh0612.hashnode.dev/"
        text="Checkout my blogs on Hashnode"
      />
        <Links
          image="https://cdn.iconscout.com/icon/free/png-512/discord-2474808-2056094.png"
          link="https://discord.gg/e3sDQjSnDK"
          text="Join my Discord server"
        />
        <Links
          image="https://upload.wikimedia.org/wikipedia/commons/thumb/9/91/Octicons-mark-github.svg/2048px-Octicons-mark-github.svg.png"
          link="https://github.com/avneesh0612"
          text="Look at my code on Github"
        />
        <Links
          image="https://www.freepnglogos.com/uploads/twitter-logo-png/twitter-logo-vector-png-clipart-1.png"
          link="https://twitter.com/AvneeshAgarwa12"
          text="Follow me on Twitter"
        />
        <Links
          image="https://www.edigitalagency.com.au/wp-content/uploads/Linkedin-logo-icon-png.png"
          link="https://www.linkedin.com/in/avneesh-agarwal-78312b20a/"
          text="Connect with me on LinkedIn"
        />
        <Links
          image="https://icons-for-free.com/iconfiles/png/512/suitcase+work+icon-1320165848716624003.png"
          link="https://avneeshresume.netlify.app/"
          text="Checkout my resume"
        />
```
These are all the links I need, feel free to add/remove/edit these links as you need.
- Remember to add the domains you use for images to ***next.config.js***. Here is all the domains I used-

```
module.exports = {
  images: {
    domains: [
      "cdn.hashnode.com",
      "upload.wikimedia.org",
      "cdn.iconscout.com",
      "www.freepnglogos.com",
      "www.edigitalagency.com.au",
      "icons-for-free.com",
    ],
  },
};
```
After doing the steps given you will see a page similar to this
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627903153861/VXWtqe646.png)

### Styling
%[https://media.giphy.com/media/NsIwMll0rhfgpdQlzn/giphy.gif]

#### Adding a background
The current background is white and boring so I will add a background image instead. I am getting an image from  [Unsplash](https://unsplash.com/) You can use something else as well.
I am choosing this image.
![background.png](https://images.unsplash.com/photo-1517707711963-adf9078bdf01?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80)

In the main div of ***index.js*** I am going to add this background image and a className of bg-cover.

```
<div
 style={{
        background:
          "url(https://images.unsplash.com/photo-1517707711963-adf9078bdf01?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1050&q=80)",
      }}
  className="flex flex-col !bg-cover items-center justify-center min-h-screen py-2"
>
```

#### Styling the links
Right now with the background we cannot see the text properly so let's add some styles to the links. I am a big fan of glassmorphic design so here is a glassmorphic link-

```
  <a
    className="flex items-center w-full max-w-md cursor-pointer p-3 my-3 mx-auto rounded-md ring-[1px] ring-blue-300 backdrop-filter backdrop-blur-2xl bg-white bg-opacity-25 shadow-xl"
    href={link}
  >
```

Adding a hover animation
```
transform hover:-translate-y-1 hover:scale-110 transition duration-200 ease-in-out
```
If you add these classes to the anchor tag you will get a nice scaling animation on hover.

%[https://www.loom.com/share/85becf4fac1345c384f36ac226cf6f7b]

Finally, we will add some styles to the text
```
<h2 className="ml-3 text-xl font-semibold md:ml-6">{text}</h2>
```

### Opening the links in another tab
Currently, the link is opening in the same tab as the app. Opening the links in a new tab will be a much better experience for the user. By adding these two attributes the link will open in a new tab.

```
    <a
      className="flex items-center w-full max-w-md cursor-pointer transform hover:-translate-y-1 hover:scale-110 transition duration-200 ease-in-out p-3 my-3 mx-auto rounded-md ring-[1px] ring-blue-300 backdrop-filter backdrop-blur-2xl bg-white bg-opacity-25 shadow-xl"
      href={link}
      rel="noreferrer"
      target="_blank"
    >
```

### Adding an avatar and name above all the links
Above all the link tags I will add this div containing my image and name.
```
      <div className="flex flex-col items-center justify-center mt-5">
        <Image
          width={200}
          height={200}
          className="mx-auto rounded-full"
          src="https://res.cloudinary.com/dssvrf9oz/image/upload/v1625825953/Avneesh_Avatar_gukdsk.png"
          alt="Avneesh Agarwal"
        />
        <h2 className="my-3 text-3xl font-bold text-center text-indigo-900 md:text-4xl">
          Avneesh Agarwal
        </h2>
      </div>
```

Congratulations! 🥳🎉 You have created your custom links app. Now you can share it with your friends, add it to your blogs, and many more.

If you managed to create your links app feel free to style it in your own way and drop the links in the comments. Here is [mine](https://avneesh-links.vercel.app).

Useful links -

[GitHub repo](https://github.com/avneesh0612/links-app)

[NextJS docs](https://nextjs.org/)

[Tailwind docs](https://tailwindcss.com/)

[Connect with me](https://avneesh-links.vercel.app)
