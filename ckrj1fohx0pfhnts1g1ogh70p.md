## How I Built ChatCube 💬 with Next.js, TailwindCSS, Clerk and Firebase and a Walkthrough of The App

### The motivation
I was exploring through Clerk the last weekend and saw this amazing hackathon that was being hosted by Clerk and Hashnode. So I thought that I should participate in it. Then one of the biggest questions was what to build?


#### How I got the idea of a chat app?
While I was thinking of what to build, I went to discord to chat a bit with my friends. At that time I thought I could build a chat app where people can have 1:1 chatting.

#### What is ChatCube?
ChatCube is an open-source 1:1 chat app that I built for the Clerk x Hashnode hackathon.

## The Tech I am using
Frontend framework: [Next.js](https://nextjs.org)

CSS: [Tailwind CSS](https://tailwindcss.com)

Authentication: [Clerk](https://clerk.dev)

Database and storage: [Firebase](https://firebase.google.com)

## Let's do a walkthrough of my app

**Check out ChatCube [here](https://www.chatcube.me)**

The first thing we would see is a login screen. Now you can sign up using your email, Google, GitHub, or Facebook. This is handled by Clerk. If you want to add Clerk to your Next.js app checkout [Mastering Clerk authentication with the Next.js standard setup](https://avneesh0612.hashnode.dev/mastering-clerk-authentication-with-the-nextjs-standard-setup)

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627179187121/CuytHk4FS.png)

#### Switching to the dark mode
If you are like me and love dark mode everywhere then it is as simple as clicking on the moon icon 🌙 in the header. ["How to Create Light and Dark Mode Toggle in Next.js with Tailwind
"](https://avneesh0612.hashnode.dev/how-to-create-light-and-dark-mode-toggle-in-next-js-with-tailwind-61e67518fd2d)

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627179225650/lHBst3yJT.png)


#### Creating a chat
In the sidebar, there is a self-explanatory create a chat button. This will open a modal and you can find a user by scrolling through it or searching 🔎. I am getting the list of users from Firebase NoSQL firestore database. This is how to add something to the database after configuring -

```
db.collection("users")
        .add(
          {
            name: "John Doe",
            email: "john@doe.com"
          },
        );
    }
```


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627207283835/LWM42lIxt.png)


#### Sending a message
To send a message just click on the name of the person you want to send the message to and type the message, and hit enter or click on the send icon. This will add the message to the collection inside the users collection.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627182538541/6rqAs3hp2.png)

#### Sending an image
If you wanna share an image with your friend click on the paper click 📎 and choose the image you wanna send. I am using firebase storage to upload all the images. After uploading the image I get a link by using which I can render them image on the screen.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627182550951/G0UeIFOOz.png)

#### Adding emojis
Everybody loves emojis so you can use your favorite emojis by clicking on the smiley face😀. Emoji mart is being used for all the emojis. If you want this feature in your app check ["How to add an emoji picker to an input field in react app
"](https://avneesh0612.hashnode.dev/how-to-add-an-emoji-picker-to-an-input-field-in-react-app)

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627182559904/JPmGzY4oC.png)

#### Using Speech instead of typing
If you are too lazy to type a message, I have got a solution for you. Click on the mic icon 🎙️ and start speaking what you wanna send and it will be converted to text.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627182585539/wkxyhAwSg.png)
P.S This feature doesn't work in the brave browser.

#### Editing and deleting the message
Editing 🖊️ and deleting a message is as simple as clicking on the three dots and selecting what you wanna do. In firebase to edit something, you need to set it to a new thing and set merge to true like this -
```
   db.collection("chats")
      .doc(router.query.id as string)
      .collection("messages")
      .doc(id)
      .set(
        {
          message: "edited message",
        },
        { merge: true }
      );
```

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627182755078/LZ8WFGQNQ.png)

#### Changing your username
If you wanna have your own unique username, click on manage account
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627120997978/jwfNo7Nrr.png)
Click on username and add/edit it.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627138239420/xDQyTst6d.png)


#### PWA
This is a Progressive Web App (PWA), So you can even install it on your device 📱💻. If you want to make your web app a PWA checkout [
How to make a Next.js app a PWA](https://medium.com/geekculture/how-to-make-a-next-js-app-a-pwa-a5e2b13da548)

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627182619436/86vx_f_OL.png)

#### Search through your chats
If you use the app for some time then you will find a lot of amazing people to chat with, so to make finding them easier you can search through them.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627182686912/pThUl75-I.png)

#### Link highliting
If you send a link 🔗 to your friend it will be highlighted and it will open in a new tab on clicking.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1627182725620/PydTTP7e_.png)


#### Clerk Production
This app is using Clerk Production where you don't have to accept third-party cookies 🍪

If you wanna contribute feel free to check out the [GitHub repository](https://github.com/avneesh0612/ChatCube) for [ChatCube](https://www.chatcube.me). You can make a PR and I will merge it soon 😉.

All the people who have contributed -
[![Contributors](https://contrib.rocks/image?repo=avneesh0612/ChatCube)](https://github.com/avneesh0612/ChatCube/graphs/contributors)

Important links -

[Live demo](https://www.chatcube.me)

[GitHub repo](https://github.com/avneesh0612/ChatCube)

[Connect with me](https://avneesh-links.vercel.app)


