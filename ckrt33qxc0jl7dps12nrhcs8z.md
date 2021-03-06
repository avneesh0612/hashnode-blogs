## How to add router progress bar in Next.js with one line of code 🤯

The pages in Next.js are lazy-loaded so if you fetch some API or fetch data from a database it can take a few seconds to reach the second page. So to let the user know that the page is being loaded we will add a progress bar at the top of the screen. Here is how it will look-


%[https://www.loom.com/share/de44131850c743dcbbefc0e4f0e48b48]


### So let's get started

#### Creating a Next app

```
npx create-next-app next-progress-bar
```


#### Installing the required dependency
```
npm i nextjs-progressbar # npm
yarn add nextjs-progressbar # yarn
```

#### Creating the pages (for demo)
First, create a new file in the pages folder. This will create a new route for you.
I am naming it ***about.js***. . I am just creating an ```h1``` tag and a ```button ``` that will redirect to the home page.

```
import Head from "next/head";
import { useRouter } from "next/router";

export default function About() {
  const router = useRouter();

  return (
    <div>
      <Head>
        <title>Create Next App</title>
        <meta name="description" content="Generated by create next app" />
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <h1>About page</h1>
      <button onClick={() => router.push("/")}>Go to Home</button>
    </div>
  );
}
```

I will do the same in ***index.js*** but redirect to the about page instead

```
import Head from "next/head";
import { useRouter } from "next/router";

export default function Home() {
  const router = useRouter();

  return (
    <div>
      <Head>
        <title>Create Next App</title>
        <meta name="description" content="Generated by create next app" />
        <link rel="icon" href="/favicon.ico" />
      </Head>

      <h1>Home page</h1>
      <button onClick={() => router.push("/about")}>Go to about me</button>
    </div>
  );
}
```

### Adding the progress bar 📊

We will go to ***_app.js*** and add the ```NextNProgress``` component

```
import "../styles/globals.css";
import NextNProgress from "nextjs-progressbar";

function MyApp({ Component, pageProps }) {
  return (
    <>
      <NextNProgress />
      <Component {...pageProps} />
    </>
  );
}

export default MyApp;
```

And that is it! The progress bar is now working 🥳🎉.

### Customizing the progress bar
You can add some props to ```NextNProgress``` like color, height, key, ref, startPosition, and many more. I mostly just change the color and height because I like the other stylings the default way they are. Here is how you can change the color and height-

```
<NextNProgress height={6} color="#4338C9" />
```

This will make the progress bar thicker and purple in color. Feel free to try out different colors and styles. Drop your favorite style for the progress bar in the comments 👇🏻

Useful links -

[Github repository](https://github.com/avneesh0612/next-progress-bar)

[NextJS docs](https://nextjs.org/docs)

[NextJS progress bar](https://www.npmjs.com/package/nextjs-progressbar)

[All socials](https://avneesh-links.vercel.app/)