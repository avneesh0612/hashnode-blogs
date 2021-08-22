## Create an infinite scrolling animation with CSS 💥

You might have seen an infinite autoscrolling animation on a website with some logos. This scrolling animation looks really good. So let's see how we can do that

![Let's do it](https://media.giphy.com/media/BpGWitbFZflfSUYuZ9/giphy.gif?cid=ecf05e47rvzs6jvgw18x08dm3o4gana6iutza5p9d5ozvs4o&rid=giphy.gif&ct=g)


** Creating a container with 6 divs inside it**

```
<div class="container">
  <div class="pink"></div>
  <div class="blue"></div>
  <div class="yellow"></div>
  <div class="orange"></div>
  <div class="purple"></div>
  <div class="aqua"></div>
</div>
```

Adding simple styles to the container and giving the divs a width, height, and different colors. You can also have images here.

```
.container {
  overflow: hidden;
  display: flex;
  height: 500px;
  width: 1500px;
  margin: 0 auto;
}

.container div {
  height: 400px;
  min-width: 300px;
  margin-left: 100px;
}

.pink {
  background-color: pink;
}

.blue {
  background-color: blue;
}

.yellow {
  background-color: yellow;
}

.orange {
  background-color: orange;
}

.purple {
  background-color: purple;
}

.aqua {
  background-color: aqua;
}
```


**Adding a keyframe -**

```
@keyframes slide {
  0% {
    transform: translate3d(0, 0, 0);
  }
  100% {
    transform: translate3d(-1500px, 0, 0); /* The image width */
  }
}
```

**Using the keyframe animation in the container**
```
.container div {
  height: 400px;
  min-width: 300px;
  margin-left: 100px;
  animation: slide 15s linear infinite;
}
```

Ta-da it was as simple as this to get it working!

%[https://www.loom.com/share/a2339e51fcf942a99de46264190e3c94]

Hope you liked this mini-tutorial, I will meet you in the next one ✌🏻

 **Useful links-**

 [Code](https://codepen.io/avneesh0612-the-bold/pen/BaZBwzq) 

[Social links](https://avneesh-links.vercel.app)
