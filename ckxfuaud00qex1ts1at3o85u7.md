## Create an animated sidebar with TailwindCSS in React💫

Hey everyone, in many apps you need a sidebar/drawer which slides in if you click on a hamburger icon. In this tutorial, we are going to see how to build that 🌟.

## Demo

%[https://www.loom.com/share/b748e5d32ebd4552aebf78be01d63408]

## Setup

Creating a new react app-

```
npx create-react-app animated-sidebar
```

### Setting up tailwindCSS

Installing Tailwind-

```
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

Configuring paths-

Inside `tailwind.config.jd` replace the content with this-

```
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

Add tailwindCSS to CSS

In `index.css` add this code block-

```
@tailwind base;
@tailwind components;
@tailwind utilities;
```


## Creating the sidebar

### Making a new component

I am going to create a separate component for Sidebar, so create a file `Sidebar.js` in the `src` folder. Now create a functional component-

```
const Sidebar = () => {
  return (
    <div>
      
    </div>
  )
}

export default Sidebar
```

### Rendering the Sidebar component

We also need to render the component so add this in `App.js`-

```
import Sidebar from "./Sidebar";

function App() {
  return (
    <div className="flex flex-col items-center justify-center min-h-screen py-2">
      <Sidebar />
    </div>
  );
}

export default App;
```

This should show us an empty canvas now.

### Making a basic sidebar

I am going to make a simple div with a text in it-

```
<div className="top-0 right-0 w-[35vw] bg-blue-600  p-10 pl-20 text-white fixed h-full ">
  <h2 className="mt-20 text-4xl font-semibold text-white">I am a sidebar</h2>
</div>
```

This will give us a simple, blue sidebar on the right side-

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1640060870893/1FSVXdoyT.png)


### Handling open and closed states

Create a useState to store a boolean value that decides if we should or shouldn't show the sidebar-

```
const [showSidebar, setShowSidebar] = useState(false);
```

We also need to show buttons/icons to open and close the sidebar so I will wrap the whole thing in a fragment, add a button to close, and a hamburger icon to open -

```
<>
  {showSidebar ? (
    <button
      className="flex text-4xl text-white items-center cursor-pointer fixed right-10 top-6 z-50"
      onClick={() => setShowSidebar(!showSidebar)}
    >
      x
    </button>
  ) : (
    <svg
      onClick={() => setShowSidebar(!showSidebar)}
      className="fixed  z-30 flex items-center cursor-pointer right-10 top-6"
      fill="#2563EB"
      viewBox="0 0 100 80"
      width="40"
      height="40"
    >
      <rect width="100" height="10"></rect>
      <rect y="30" width="100" height="10"></rect>
      <rect y="60" width="100" height="10"></rect>
    </svg>
  )}

  <div className="top-0 right-0 w-[35vw] bg-blue-600  p-10 pl-20 text-white fixed h-full z-40">
    <h3 className="mt-20 text-4xl font-semibold text-white">I am a sidebar</h3>
  </div>
</>
```

This will not make any difference right now but let's add some conditional classes to the main sidebar div.

```
<div
  className={`top-0 right-0 w-[35vw] bg-blue-600  p-10 pl-20 text-white fixed h-full z-40 ${
    showSidebar ? "translate-x-0 " : "translate-x-full"
  }`}
```

If the `showSidebar` variable is true then it will add the `translate-x-0` otherwise `translate-x-full`. Our sidebar now works 🎉

%[https://www.loom.com/share/c9ddedafa8ca4cf7873f930f4aa52863]

But it isn't smooth so let us see how to make the animation smooth. Just add these two classes to the blue div-

```
ease-in-out duration-300
```

The div should look like this now-

```
<div
  className={`top-0 right-0 w-[35vw] bg-blue-600  p-10 pl-20 text-white fixed h-full z-40  ease-in-out duration-300 ${
    showSidebar ? "translate-x-0 " : "translate-x-full"
  }`}
>
  <h3 className="mt-20 text-4xl font-semibold text-white">I am a sidebar</h3>
</div>
```

Our sidebar animation looks very smooth and great! 🥳

%[https://www.loom.com/share/b748e5d32ebd4552aebf78be01d63408]

Hope you liked this tutorial and add nice animation to the sidebar in your project. Peace ✌️


## Useful links

[GitHub repo](https://github.com/avneesh0612/animated-sidebar)

[Animate and Change Header Background on Scroll](https://blog.avneesh.tech/animate-and-change-header-background-on-scroll)

[Connect with me](https://links.avneesh.tech/)