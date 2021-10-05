## How to Create a PWA With Next.js

Hey all,

This post is in collaboration with @[James Q Quick](@jamesqquick). If you like to see videos then check out this video by  [James Q Quick](https://www.youtube.com/c/JamesQQuick).

%[https://youtu.be/ARNN_zmrwcw]


So let's get the party started.
 
### What is a PWA?

PWA stands for Progressive web app. It is basically like a web app built with HTML, CSS, and Javascript with a few more features like-

* Extremely Fast
* Installable
* Works in all browsers
* Provides an offline page
* Push notifications

## Setup
### Creating a Next.js app

```
npx create-next-app next-pwa-demo
```

I am going to convert the default Next.js template into a PWA, you can convert your web app.

### Installing the required dependency
```
npm i next-pwa # npm
yarn add next-pwa # yarn
```

## Generating a manifest
I am going to use [Simicart ](https://www.simicart.com/manifest-generator.html) for generating the manifest. You can simply add the details of your app and it will generate a manifest. Make sure to select `standalone` in the display

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631105021139/isB5mFO9g.png)

The generated manifest will look similar to this

```
{
  "theme_color": "#000000",
  "background_color": "#ffffff",
  "display": "standalone",
  "scope": "/",
  "start_url": "/",
  "name": "Next PWA",
  "short_name": "PWA",
  "description": "This app is to demo PWA in Next.js",
  "icons": [
    {
      "src": "/icon-192x192.png",
      "sizes": "192x192",
      "type": "image/png"
    },
    {
      "src": "/icon-256x256.png",
      "sizes": "256x256",
      "type": "image/png"
    },
    {
      "src": "/icon-384x384.png",
      "sizes": "384x384",
      "type": "image/png"
    },
    {
      "src": "/icon-512x512.png",
      "sizes": "512x512",
      "type": "image/png"
    }
  ]
}
```

After you are done with adding all the details, install the zip file and extract it. After you have extracted, drag and drop all the files in the public folder.
 We will rename `manifest.webmanifest` to `manifest.json`.

Finally, the public directory should look like this


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631104061093/p46yEEcg8.png)

### Creating  `_document.js`

Create `_document.js` in the pages folder and add the following piece of code

```
import Document, { Html, Head, Main, NextScript } from "next/document";

class MyDocument extends Document {
  render() {
    return (
      <Html>
        <Head>
          <link rel="manifest" href="/manifest.json" />
          <link rel="apple-touch-icon" href="/icon.png"></link>
          <meta name="theme-color" content="#fff" />
        </Head>
        <body>
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default MyDocument;
```

### Configuring PWA in Next config

We will need to add some data for configuring the PWA, so add this snippet in `next.config.js`.

```
const withPWA = require("next-pwa");

module.exports = withPWA({
  pwa: {
    dest: "public",
    register: true,
    skipWaiting: true,
  },
});
```

You have made your app a PWA 🥳

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631107349883/VTP2RgjEd.png)

### Making better development environment


#### Adding the auto-generated files to .gitignore

If you notice then a few files like service workers, and workbox has been added to the public folder. 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631106642797/L1CYGDmvD.png)


These files change constantly and are not needed in your GitHub. So, do the following to remove them from production.

* Delete `sw.js`, `sw.js.map`, `workbox-****.js` and `workbox-****.js.map`.

* Add the files to `.gitignore`

```
# PWA files
**/public/sw.js
**/public/workbox-*.js
**/public/worker-*.js
**/public/sw.js.map
**/public/workbox-*.js.map
**/public/worker-*.js.map
```

* Now if you restart the server then the filenames will be dark

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631106954234/TB-1YKiLR.png)


#### Disabling PWA in development
In development, you might want to disable PWA because it gives a lot of console messages.

We will disable it like this in `next.config.js`

```
const withPWA = require("next-pwa");

module.exports = withPWA({
  pwa: {
    dest: "public",
    register: true,
    skipWaiting: true,
    disable: process.env.NODE_ENV === "development",
  },
});
```

Hope you found this useful. See ya in the next one ✌🏻

**Useful links-**

[James's video](https://youtu.be/ARNN_zmrwcw) 

[Follow James on Twitter](https://twitter.com/jamesqquick)

[All socials](https://avneesh-links.vercel.app/)
