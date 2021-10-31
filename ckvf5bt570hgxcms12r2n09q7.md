## Create a custom video player in React 📽️

Wassup guys, in this tutorial we are going to see how to build a custom video player in React. Let's jump straight into it!

![jump right in](https://c.tenor.com/ZuUyAjVssKoAAAAC/so-lets-just-get-right-into-it-hannah-fawcett.gif)

## Setup

### Create a new react app

```
npx create-react-video-app custom-video-player
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


## Create the UI for our Video player

### Adding the Video
Inside the app div add a video tag with the src of your video, I am also going to add a className for styling-

 ``` 
 <video
    className="video"
    src="https://res.cloudinary.com/dssvrf9oz/video/upload/v1635662987/pexels-pavel-danilyuk-5359634_1_gmixla.mp4"
 ></video>
```

### Adding the controls of the videos
Below the video component, I will add this div which has some Svgs as icons. You can use direct Svgs like me or use an icon library for the icons :).

```
  <div className="controlsContainer">
        <div className="controls">
          <img className="controlsIcon" alt="" src="/backward-5.svg" />
          <img className="controlsIcon--small" alt="" src="/play.svg" />
          <img className="controlsIcon" alt="" src="/forward-5.svg" />
        </div>
  </div>
```

### Adding the progress bar for time
We are also going to create a progress bar that shows the current time and total time of the video.

```
 <div className="timecontrols">
        <p className="controlsTime">1:02</p>
        <div className="time_progressbarContainer">
          <div style={{ width: "40%" }} className="time_progressBar"></div>
        </div>
        <p className="controlsTime">2:05</p>
   </div>
```


> Forgive me for not having very good naming conventions in classes. *I have forgotten how to name my classes because of Tailwind :P*


## Styling the UI
The video player looks very ugly right now, so let's style it. In *App.css* I am going to add some stylings-

```
/* Main Container */

.app {
  display: flex;
  flex-direction: column;
  width: 100vw;
  height: 100vh;
  overflow: hidden;
}

/* Video */

.video {
  width: 100vw;
  height: 100vh;
}

/* Controls */

.controlsContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100vw;
  background-color: transparent;
  margin-top: -50vw;
  padding: 0 40px;
  z-index: 20;
}

.controls {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  padding-top: 18rem;
  margin: auto;
}

.controlsIcon {
  width: 40px;
  height: 40px;
  cursor: pointer;
  margin-left: 10rem;
  margin-right: 10rem;
}

.controlsIcon--small {
  width: 32px;
  height: 32px;
  cursor: pointer;
  margin-left: 10rem;
  margin-right: 10rem;
}

/* The time controls section */

.timecontrols {
  display: flex;
  align-items: center;
  justify-content: space-evenly;
  position: absolute;
  bottom: 4rem;
  margin-left: 10vw;
}

.time_progressbarContainer {
  background-color: gray;
  border-radius: 15px;
  width: 75vw;
  height: 5px;
  z-index: 30;
  position: relative;
  margin: 0 20px;
}

.time_progressBar {
  border-radius: 15px;
  background-color: indigo;
  height: 100%;
}

.controlsTime {
  color: white;
}
```

Now our video player would look like this-

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1635672034793/swelNl_sp.png)

## Adding the logic to the player
To work on the functionalities we first need to attach a ref to the video with the useRef hook. So follow the steps given below:

- Create a ref like this-

```  
const videoRef = useRef(null);
```

- Import the useRef hook from react

```
import { useRef } from "react";
```

- Attach it to the video

```
 <video
    ref={videoRef}
    className="video"
    src="https://res.cloudinary.com/dssvrf9oz/video/upload/v1635662987/pexels-pavel-danilyuk-5359634_1_gmixla.mp4"
 ></video>
```

### Play and Pause functionality
For play and pause create a simple function, which takes an argument of control and based on the control it will play or pause the video-

```
 const videoHandler = (control) => {
    if (control === "play") {
      videoRef.current.play();
    } else if (control === "pause") {
      videoRef.current.pause();
    }
  };
```

Now in the play.svg image, we will add an onClick function to start the video.
```
  <img
     onClick={() => videoHandler("play")}
     className="controlsIcon--small"
     alt=""
     src="/play.svg"
     />
```
If you click on the icon the video will play!

**Changing the icon based on the playing/paused state**

To achieve this I am going to use the useState hook. Create a playing state like this-

```
const [playing, setPlaying] = useState(false);
```
In the const video handler function, we need to change the value onClick of them like this-

```
const videoHandler = (control) => {
    if (control === "play") {
      videoRef.current.play();
      setPlaying(true);
    } else if (control === "pause") {
      videoRef.current.pause();
      setPlaying(false);
    }
  };
```

**Changing the icon**
Where we have the play icon, now we will render it based on a condition with the help of a ternary operator -

```
  {playing ? (
            <img
              onClick={() => videoHandler("pause")}
              className="controlsIcon--small"
              alt=""
              src="/pause.svg"
            />
          ) : (
            <img
              onClick={() => videoHandler("play")}
              className="controlsIcon--small"
              alt=""
              src="/play.svg"
            />
          )}
```


Now, we can play and pause the video 🥳


### Forwarding and reverting the video

I am going to create very simple functions for this-

```
 const fastForward = () => {
    videoRef.current.currentTime += 5;
  };

  const revert = () => {
    videoRef.current.currentTime -= 5;
  };
```

Now we will add these functions as onClick of the respective buttons.

**Forward**

```
<img
  onClick={fastForward}
  className="controlsIcon"
  alt=""
  src="/forward-5.svg"
     />
```

**Revert**

```
<img
  onClick={revert}
  className="controlsIcon"
  alt=""
  src="/backward-5.svg"
     />
```


### Time progress bar
**Get the length of the video**

To get the length of the video, follow the following steps

- Give an id to the video component

```
 <video
    id="video1"
    ref={videoRef}
    className="video"
    src="https://res.cloudinary.com/dssvrf9oz/video/upload/v1635662987/pexels-pavel-danilyuk-5359634_1_gmixla.mp4"
 ></video>
```

- Create a state to store the video length

```
const [videoTime, setVideoTime] = useState(0);
```

- Set the video time like this on the play of the video

```
if (control === "play") {
      videoRef.current.play();
      setPlaying(true);
      var vid = document.getElementById("video1");
      setVideoTime(vid.duration);
    }
```

Now we can use the videoTime variable instead of hardcoded time. This string manipulation will make the time in a format like- 1:05

```
  <p className="controlsTime">
    {Math.floor(videoTime / 60) + ":" + ("0" + Math.floor(videoTime % 60)).slice(-2)}
 </p>
```

**Getting the current time of the video**

To get the current time of video we will need to use a function that runs every second, so I am going to use window.setInterval for the same.

```
window.setInterval(function () {
    setCurrentTime(videoRef.current?.currentTime);
  }, 1000);
```

Now as always, we need to create a state to store the value-

```
const [currentTime, setCurrentTime] = useState(0);
```

Instead of the hard code value, we will use the variable

```
<p className="controlsTime">
    {Math.floor(currentTime / 60) + ":" + ("0" + Math.floor(currentTime % 60)).slice(-2)}
</p>
```

**Getting the progress and setting it to the progress bar**

Create another state for progress-

```
const [progress, setProgress] = useState(0);
```

Now inside the window.setInterval function that we created, add another line-

```
setProgress((videoRef.current?.currentTime / videoTime) * 100);
```

The function would look like this now-

```  
window.setInterval(function () {
    setCurrentTime(videoRef.current?.currentTime);
    setProgress((videoRef.current?.currentTime / videoTime) * 100);
  }, 1000);```


**Our custom video player is now ready 🎉🎊**


**Useful links-**

[GitHub repository](https://github.com/avneesh0612/custom-video-player) 

[ReactJS docs](https://reactjs.org/)

[All socials](https://avneesh-links.vercel.app/)


