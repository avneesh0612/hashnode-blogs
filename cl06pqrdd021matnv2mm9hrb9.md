## How to make a count down timer in React 🔥

Hello everyone, in many kinds of apps you need to build a count down. So, today we will see how to build a countdown timer in React!

![Tick tock](https://c.tenor.com/43Q-hz9sRGoAAAAC/clock-watching.gif)


## Setup

### Create a new react app

```
npx create-react-app react-countdown
```

### Cleanup
- Delete everything in the app div in `App.js`.

```
import "./App.css";

function App() {
  return <div className="app"></div>;
}

export default App;
```

- Delete everything in `App.css`

- in `index.css` add-

```
* {
  margin: 0;
}
```

### Starting the app
To start your react app run the following commands

```
npm run start # npm

yarn start # yarn
```

If you now open [localhost:3000](http://localhost:3000/), it should show you an empty canvas to work with.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1646050843709/RHBtj3sTB.png)


## Creating the countdown timer
Inside `App.js` create a new function called `calculateTimeLeft`-

```
  const calculateTimeLeft = () => {

  };
```

Let's now create this function! Inside the function add a new variable called `difference`-

```
const difference = +new Date("2022-02-28T18:30:00+05:30") - +new Date();
```

Add the end date with time and timezone inside the first new Date constructor. The one here is "28th February 2022 18:30 IST". You can generate this from [Time stamp generator](https://timestampgenerator.com/). I would recommend using the "W3C" format.

![confusion](https://media2.giphy.com/media/DHqth0hVQoIzS/giphy.gif)

Now, inside the function create a new variable to store the time-

```
let timeLeft = {};
```

Now let's write the logic for calculating time left-

```
   if (difference > 0) {
      timeLeft = {
        hours: Math.floor(difference / (1000 * 60 * 60)),
        minutes: Math.floor((difference / 1000 / 60) % 60),
        seconds: Math.floor((difference / 1000) % 60),
      };
    }

    return timeLeft;
```

This is just a basic division for calculating the time in hours, minutes, and seconds.


Now, create a new state for storing the time and a useEffect for updating it very second-

```
 const [timeLeft, setTimeLeft] = useState(calculateTimeLeft());

  useEffect(() => {
    setTimeout(() => {
      setTimeLeft(calculateTimeLeft());
    }, 1000);
  });
```

You will also need to import`useState` and `useEffect`-

```
import { useEffect, useState } from "react";
```

Finally, let's render the time-

```
return (
    <div className="App">
      <p>
        <span>{timeLeft.hours}</span>
        <span>:</span>
        <span>{timeLeft.minutes}</span>
        <span>:</span>
        <span>{timeLeft.seconds}</span>
      </p>
    </div>
  );
```

This is going to take the time in hours, minutes, and seconds from the timeLeft object.
Our timer is now successfully working 🥳


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1646052511439/npEQUocwX.png)


## Do something once the countdown is over
Once the countdown is over we might want to show something else to the user. We can do this by simply checking if `timeLeft.hours` or `timeLeft.minutes` or `timeLeft.seconds` exist-

```
{timeLeft.hours || timeLeft.minutes || timeLeft.seconds ? (
    <p>
      <span>{timeLeft.hours}</span>
      <span>:</span>
      <span>{timeLeft.minutes}</span>
      <span>:</span>
      <span>{timeLeft.seconds}</span>
    </p>
  ) : (
    <p>Time is up 🔥</p>
  );
}
```
If you now set the date to a time that has passed, you can see that it shows Time is up!


## Conclusion
Making a countdown timer in react is this easy with react hooks! Hope you could make a countdown timer and learned from this tutorial. See ya in the next one ✌️


### Useful links

[GitHub repo](https://github.com/avneesh0612/React-countdown-tutorial)

[More about react hooks](https://reactjs.org/docs/hooks-intro.html)

[Let's connect](https://links.avneesh.tech/)