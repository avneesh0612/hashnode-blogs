## Easy smooth scrolling in react

In a single-page web application, you will probably have a navbar allowing the user to go to different sections of the page. So today we are going to see how to build that.


![image.png](https://c.tenor.com/rSTlY_W4GzAAAAAC/fresh-prince-of-bel-air-carlton.gif)


## Demo
%[https://www.loom.com/share/1862dfd99f7249a59913db2c9dd62062]


## Setup

### Creating a new react app

```
npx create-react-app react-scroll-demo
```

### Cleanup

* Delete everything inside `App.css`
* Delete the content of the App div in `App.js`

### Starting the app

```
yarn start # yarn
npm start # npm
```

### Creating the different sections
Inside App.js, I will create 4 divs with different ids like this

```
import "./App.css";

function App() {
  return (
    <div className="App">
      <div id="section1">
        <h1>Section 1</h1>
      </div>
      <div id="section2">
        <h1>Section 2</h1>
      </div>
      <div id="section3">
        <h1>Section 3</h1>
      </div>
      <div id="section4">
        <h1>Section 4</h1>
      </div>
    </div>
  );
}

export default App;
```

You will see 4 h1 tags like this

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631443914431/-9neN8AMc.png)

### Styling the sections
I am going to apply some basic stylings to the sections

```
.App {
  text-align: center;
}

.App > div {
  width: 100vw;
  min-height: 100vh;
  margin-top: 100px;
}
```
This will center the text and give a height and width of the screen to the sections.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1631450141219/hhnkAYToe.png)


### Creating the header
Create `header.js` and `header.css` in the src folder.

We will create a simple navbar with 4 nav items in it

```
import "./Header.css";

const Header = () => {
  return (
    <nav>
      <ul className="header">
        <li>Section 1</li>
        <li>Section 2</li>
        <li>Section 3</li>
        <li>Section 4</li>
      </ul>
    </nav>
  );
};

export default Header;
```

### Styling the header
I have added some simple stylings so that the header looks better. So add these styles in header.css.

```
.header {
  display: flex;
  justify-content: space-around;
  width: 100%;
  padding: 20px 0;
  position: fixed;
  background-color: aqua;
  top: 0;
}

li {
  cursor: pointer;
}
```


### Rendering the header
Inside the app div add the header component and import it

```
import "./App.css";
import Header from "./Header";

function App() {
  return (
    <div className="App">
      <Header />
      <div id="section1">
        <h1>Section 1</h1>
      </div>
      <div id="section2">
        <h1>Section 2</h1>
      </div>
      <div id="section3">
        <h1>Section 3</h1>
      </div>
      <div id="section4">
        <h1>Section 4</h1>
      </div>
    </div>
  );
}

export default App;
```


### Creating the smooth scroll

![image.png](https://c.tenor.com/xRYeFbuVpEcAAAAd/the-moment-everybody-was-waiting-for-dean-schneider.gif)

#### Installing the dependencies
```
yarn add react-scroll # yarn
npm i react-scroll # npm
```
Now, inside the list items add the Link component and a few peops with it like this

```
   <li>
    <Link
      activeClass="active"
      to="section1"
     spy={true}
     smooth={true}
     offset={-100}
     duration={500}>
         Section 1
     </Link>
   </li>
```

You need to add the id of the section you want to be able to scroll to in `to`. The offset is the distance to be left while scrolled. Feel free to mess around and make some changes to it, to suit you the best.

I have added the links to all the sections and it looks like this

```
import { Link } from "react-scroll";
import "./Header.css";

const Header = () => {
  return (
    <nav>
      <ul className="header">
        <li>
          <Link
            activeClass="active"
            to="section1"
            spy={true}
            smooth={true}
            offset={-100}
            duration={500}
          >
            Section 1
          </Link>
        </li>
        <li>
          <Link
            activeClass="active"
            to="section2"
            spy={true}
            smooth={true}
            offset={-100}
            duration={500}
          >
            Section 2
          </Link>
        </li>
        <li>
          <Link
            activeClass="active"
            to="section3"
            spy={true}
            smooth={true}
            offset={-100}
            duration={500}
          >
            Section 3
          </Link>
        </li>
        <li>
          <Link
            activeClass="active"
            to="section4"
            spy={true}
            smooth={true}
            offset={-100}
            duration={500}
          >
            Section 4
          </Link>
        </li>
      </ul>
    </nav>
  );
};

export default Header;
```

Hope you successfully managed to add smooth scrolling to your react app. If you have any queries then shoot them in the comments below 👇🏻. See ya in the next one ✌🏻

**Useful links-**

[Github Repo](https://github.com/avneesh0612/react-scroll)

[React scroll](https://www.npmjs.com/package/react-scroll)

[All socials](https://avneesh-links.vercel.app/)