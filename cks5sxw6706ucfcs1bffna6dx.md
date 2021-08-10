## What is the package.json and How to Update all Packages in your package.json with only 2 commands 😲

Hey everyone 👋🏻, I get this question many times that what is the use of package.json, and what to do if our versions of packages are older? So here is an answer to both the questions

If you were working on an app a long time back and want to update some things, you should update the versions of the packages. Doing this manually will take a lot of time, so today I am gonna show you how to do this with only 2 short commands.

## What is package.json?
Whenever you add a package with ``npm install package-name`` or ``yarn add package-name``. Then the package gets added in the package.json. So basically, it is the list of packages that are present in your app.

## Why should you update a package?
* There might be a new feature in some of the packages you want to use, but it wasn't there when you installed it.
* The syntax might have changed.
* It keeps your app stable, usable, and secure.
* The package would have improved.
* Bugs would be removed (if any)

## Updating the packages
So today, I am just gonna clone an old project of mine and show the difference between the two package.json files.

These are the dependencies in the ```package.json``` file of an old react project-
```
 "dependencies": {
    "@dnd-kit/core": "^1.0.2",
    "@testing-library/jest-dom": "^5.11.4",
    "@testing-library/react": "^11.1.0",
    "@testing-library/user-event": "^12.1.10",
    "aos": "^3.0.0-beta.6",
    "emailjs-com": "^2.6.4",
    "framer-motion": "^3.10.0",
    "react": "^17.0.1",
    "react-dom": "^17.0.1",
    "react-scripts": "4.0.1",
    "react-typewriter": "^0.4.1",
    "web-vitals": "^0.2.4"
  },
```
### Running the two magical 🪄 commands 

```
npx npm-check-updates -u
yarn install
```


This is what the updated package.json looks like-
```
 "dependencies": {
    "@dnd-kit/core": "^3.1.1",
    "@testing-library/jest-dom": "^5.14.1",
    "@testing-library/react": "^12.0.0",
    "@testing-library/user-event": "^13.2.1",
    "aos": "^3.0.0-beta.6",
    "emailjs-com": "^3.2.0",
    "framer-motion": "^4.1.17",
    "react": "^17.0.2",
    "react-dom": "^17.0.2",
    "react-scripts": "4.0.3",
    "react-typewriter": "^0.4.1",
    "web-vitals": "^2.1.0"
  },
```
A new version has come for almost every package

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1628143690871/mgQCt1KP-.png)

I hope you learned from this short article. Let me know what you want to see next 😉.

Useful links -

[More about Package.json](https://docs.npmjs.com/cli/v7/configuring-npm/package-json)

[Connect with me](https://avneesh-links.vercel.app)
