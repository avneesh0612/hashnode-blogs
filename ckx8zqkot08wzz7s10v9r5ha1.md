## Next SEO: A better way to Manage SEO for Next.js 🔍

The Next.js head tags are a good way to add the meta tags, title, description, open graph image, etc. but you might not remember all the meta tags and it can also look messy, so we are going to see how to simplify this process using a package called  [next seo](https://www.npmjs.com/package/next-seo).

![](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcQSbuvi5L1jobUfUGX6vQH4esEeKJMjATzx6V9mLqTWW04G8C8ie3AZs1L5ChjFGjQ74TQ&usqp=CAU)

## Installing the package
As it is an external library we need to install it-

```
npm i next-seo
```


## Using next SEO

I like to add all the properties in `_app.js` so it automatically works on all pages and to modify some data for pages I add it to that page. Now let's see how to use it. Inside `_app.js`, in the return block add this-

```
   <NextSeo
        title="Avneesh Agarwal"
        titleTemplate="Avneesh Agarwal"
        defaultTitle="Avneesh Agarwal"
        description="A full stack web developer, who loves to design and develop beautiful websites. I have been coding for over a year now. One of my hobbies is writing, I love to document my journey by writing blog posts and also teach others through them."
        canonical="https://www.avneesh.tech/"
        openGraph={{
          url: "https://www.avneesh.tech/",
          title: "Avneesh Agarwal",
          description: "A full stack web developer, who loves to design and develop beautiful websites. I have been coding for over a year now. One of my hobbies is writing, I love to document my journey by writing blog posts and also teach others through them.",
          images: [
            {
              url: "/og-image.png",
              width: 800,
              height: 420,
              alt: "Avneesh Agarwal",
            },
          ],
        }}
        twitter={{
          handle: "@avneesh0612",
          site: "@avneesh0612",
          cardType: "summary_large_image",
        }}
      />
```

If you didn't have a wrapper/fragment then you need to wrap this and `<Component {...pageProps} />` like-

```
   <>
       <NextSeo
        title="Avneesh Agarwal"
        titleTemplate="Avneesh Agarwal"
        defaultTitle="Avneesh Agarwal"
        description="A full stack web developer, who loves to design and develop beautiful websites. I have been coding for over a year now. One of my hobbies is writing, I love to document my journey by writing blog posts and also teach others through them."
        canonical="https://www.avneesh.tech/"
        openGraph={{
          url: "https://www.avneesh.tech/",
          title: "Avneesh Agarwal",
          description: "A full stack web developer, who loves to design and develop beautiful websites. I have been coding for over a year now. One of my hobbies is writing, I love to document my journey by writing blog posts and also teach others through them.",
          images: [
            {
              url: "/og-image.png",
              width: 800,
              height: 420,
              alt: "Avneesh Agarwal",
            },
          ],
        }}
        twitter={{
          handle: "@avneesh0612",
          site: "@avneesh0612",
          cardType: "summary_large_image",
        }}
      />
      <Component {...pageProps} />
    </>
```

You will also need to import it-

```
import { NextSeo } from "next-seo";
```


### What each of the props does


- title: This is the title of the webpage that you can see in the browser and when you share it as a link

- titleTemplate: title template is the same as the title

- defaultTitle: If you don't provide any title on some page, then this will be used there.

- description: This is the description of the site which helps search engines find the websites and is also shown when you share the URL.

- canonical: This should be the domain of your website in most cases but suppose you are republishing a post then this would link to the original post.

- openGraph: This takes in an object of data like title, description, image. This data will be used to show when you share these links on discord, Twitter, Facebook, WhatsApp, Slack, etc.

- Twitter: Here you can define your username, site, and how the card should look. In most cases `summary_large_image` works fine.


## Changing data based on pages

Suppose you want to change the title or description of a page, you can add in the `NextSeo` with the tags you want to change. For example, I want to change the title to `About Me`, I will add this to the `about.js` page-

```
<NextSeo title="About Me" />
```

You can pass in as many props depending on what you need to change :D.

## Conclusion

Hope you found this article useful. See ya next time ✌️


### Useful links

[Next SEO package](https://www.npmjs.com/package/next-seo)

[Next.js SEO course](https://nextjs.org/learn/seo/introduction-to-seo)

[Connect with me](https://links.avneesh.tech/)