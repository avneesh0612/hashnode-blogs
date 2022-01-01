## Stop showing that boring 404 page in your Next.js app!

You might have seen some fancy 404 pages and liked it! In this article let's see how we can make one too.

## Default 404 page with Next

If you go to a route that doesn't exist in your Next.js app you will see this simple 404 page-

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641043048247/kZ_PNdQOl.png)


This is okay but let's see how to improve/customize it 😎.


## Creating a custom 404 page

Inside the `pages` folder create a new file `404.js`. Here, you can create a page just like you have normally done for other pages. I have created this simple React component-

```
import styles from "../styles/404.module.css";

const customError = () => {
  return (
    <div className={styles.container}>
      <h1>Why are you here?</h1>

      <a href="https://www.youtube.com/watch?v=dQw4w9WgXcQ">
        <img
          src="https://i.ytimg.com/vi/dQw4w9WgXcQ/hqdefault.jpg"
        />
      </a>
    </div>
  );
};

export default customError;
```

I have also added some basic styling in a new file, `404.module.css`-

```
.container {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  height: 100vh;
  width: 100vw;
}
```

This gives us a nice 404 page! :D

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641048342750/Y_YqMtqNt.png)



## Conclusion

Next.js makes it pretty easy to customize the 404 page. This was a short, simple tutorial. See you in the next one ✌️

### Useful links

[GitHub repo](https://github.com/avneesh0612/next-404)

[Next.js](https://nextjs.org/)  

 [Connect with me](https://links.avneesh.tech/) 