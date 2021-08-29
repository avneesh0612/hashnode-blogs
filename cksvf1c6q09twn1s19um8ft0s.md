## Introducing Voyagger - Connecting people, Changing lives

Hey all, during August I worked on  [**Voyagger**](https://www.voyagger.tech/) for the Auth0 x @[Hashnode](@hashnode) hackathon.

### Inspiration
When I read about this competition I started brainstorming ideas when it struck me- A hassle-free delivery service. These tough times have made us all aware of the importance of our loved ones and through this app, users can bring a smile to their family and friends' faces by sending them their favorite delicacy, medicines, or a simple heartfelt gift.
### The Tech 💻 I am using

Framework:  [Next.js](https://nextjs.org) 

CSS:  [Tailwind CSS](https://tailwindcss.com) 

Authentication:  [Auth0](https://auth0.com)

Database:  [Firebase](https://firebase.google.com) 

Payment:  [Stripe](http://stripe.com) 

State management:  [Redux](https://redux.js.org) 

Animations:  [Framer Motion](https://www.framer.com/motion)


### Authentication 🔒
To use most of the services in the app, you will need to be authenticated first. The authentication system is built with [Auth0](https://auth0.com). If you want to add auth0 authentication to your app checkout ["Add auth0 authentication to your Next.js app in less than 5 🖐🏻 minutes"](https://avneesh0612.hashnode.dev/add-auth0-authentication-to-your-nextjs-app-in-less-than-5-minutes) 

**Biometrics**

I have added a biometrics login where you can log in through your fingerprint or other biometrics. This is a feature present in Auth0.


### Order food 😋

All the food categories and items have been stored in the database instead of hardcoding them so that none can manipulate the data like the price of the product.

You can select the food items you want from the various categories and buy them after adding them to the cart on the order food page.

How it works-

* Every time you hit the plus icon it will add the product to the global redux store for accessing the data in different components and then to local storage for the persistence of the items.

* If you click on the checkout button then it will create a checkout session with stripe. If you want to learn more about Stripe then checkout  ["Accept payments through Stripe in a Next.js app"](https://avneesh0612.hashnode.dev/payments-in-next). For testing the checkout you can use the card `4242 4242 4242 4242` with random details.

* After you have completed the payment, a Stripe webhook will be triggered which will add the order details to the database, which is Firebase in this case.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1629441880325/Dudi8Q_C8.png)


After placing the order you can view all the placed items on the order page.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1629444741707/rngQbBJqi.png)

### Send a parcel 📦
The sending parcel page will show you a simple form where you need to add a few details required for pick up and delivery of the parcel like your address, the recipient's address, phone number, etc.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1629446683505/cZXJ7eJrK.png)

When you submit the form it will add the data to Firebase. Adding data to the Firebase firestore database is as simple as doing this

```
 db.collection("parcels").add({
      pickupaddress: "your address",
      zip: "zip code",
      weight: "weight of the package",
    });
```
Now you can see all the parcels you have added, on the parcel tracking page. If you want to share the details with a friend then simply click "Share with someone" and the link will be copied.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1629446716708/gXMT6L66C.png)

 You might think that copying the link might be a hard task but it is a simple one-liner

```
navigator.clipboard.writeText("Text you want to copy");
```

### Books 📚
I am using the [Google books API](https://developers.google.com/books/docs/v1/using) to get so many books. Here is an example endpoint to get all the books-

```
https://www.googleapis.com/books/v1/volumes?q=Fiction
```


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1629447329742/3EG9EEw3_.png)


### Order medicines 💊
The order medicines page is very similar to the order food page, the only difference being in the products.
![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1629451017517/BL1Vuwmwi.png)

### PWA
This is a Progressive Web App (PWA), So you can even install it on your device 📱💻. If you want to make your web app a PWA checkout [
How to make a Next.js app a PWA](https://medium.com/geekculture/how-to-make-a-next-js-app-a-pwa-a5e2b13da548)


### Performance and good practices
Using  [ESlint ](https://eslint.org) with eslint-config-next for better linting.

Using  [Typescript](https://www.typescriptlang.org) 

**Lighthouse scores**

When not signed in -

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1629359598474/8xSP_FuaR.png)

When signed in -

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1629359576520/zaG-LiBty.png)


If you wanna contribute, feel free to check out the [GitHub repository](https://github.com/avneesh0612/voyagger) for [Voyagger](https://www.voyagger.tech/)

Important links -

[Live demo](https://www.voyagger.tech/)

[GitHub repo](https://github.com/avneesh0612/voyagger)

[Connect with me](https://avneesh-links.vercel.app)
