## TailwindCSS v3 released! A look at the new features 🎨

## Intro

Tailwind CSS v3.0 just got released and they have added a lot of cool features, so let's have a look at them.

![](https://media2.giphy.com/media/AlGNpyW6meDD21cx0l/200w.gif?cid=82a1493b3u5ll141qgl3ry8ipjsig5g9xkijm0mumnae32xr&rid=200w.gif&ct=g)

## JIT is now inbuilt

The JIT mod is now inbuilt in TailwindCSS so it is way faster now and also enables some cool new features like stackable variants, arbitrary value support.

![](https://media1.giphy.com/media/3ornjIhZGFWpbcGMAU/200.gif)

## Lot's of new colors
There are many new colors now and they are 22 in total! 

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1639238763636/cRjvElbWl.png)

There are also 5 different gray's now! 🤯

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1639238828976/7wkTkFmFE.png)


## Colored box-shadow
Colored box-shadow was an important feature that wasn't in tailwind until v3. But it is finally here and so easy to use :D

```
<button class="shadow-lg shadow-green-500 text-center bg-green-500 text-white px-8 py-3 rounded-full text-xl">Button</button>
```

A few classes can you give you this beautiful button-

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1639294864875/XTrLR9Lql.png)


## Aspect ratio

Now you don't have to use any plugins to use the aspect class, you can use the various types provided like- `aspect-video`, `aspect-auto`, `aspect-square`, or use custom values like `aspect-[9/6]` as JIT is now inbuilt.


## Scroll snap

This is a great feature that allows you to have a list of scrollable elements which snap to center, start, or end. In this demo, I am using center snap-

```
<div class="snap-x flex overflow-scroll">
  <img
    class="mx-5 snap-center w-[1000px]"
    src="https://images.unsplash.com/photo-1604537466158-719b1972feb8?ixlib=rb-1.2.1&ixid=MnwxMjA3fDF8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1169&q=80"
  />
  <img
    class="mx-5 snap-center w-[1000px]"
    src="https://images.unsplash.com/photo-1469474968028-56623f02e42e?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1174&q=80"
  />

  <img
    class="mx-5 snap-center w-[1000px]"
    src="https://images.unsplash.com/photo-1444464666168-49d633b86797?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1169&q=80"
  />
  <img
    class="mx-5 snap-center w-[1000px]"
    src="https://images.unsplash.com/photo-1559213237-6fdea41b7308?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1170&q=80"
  />
</div>
```

This will give a nice snap scrolling effect like this

%[https://www.loom.com/share/f6bc3808f1044488ad33e2c4a5424f82]

## Scroll behavior

You can now add smooth scrolling in tailwind CSS by adding the `scroll-smooth` class. If you want to add margin to some places for scroll, it can be done by prefixing scroll, like- `scroll-m-4`.

## Multi-column layout

Tailwind now supports multi-column/newspaper-like layouts! You can give a specific number of columns you need or just set it to auto for the browser to decide how many columns to use based on size.


## Accent color & file upload

**Accent color**

You can now customize your forms to match the theme of your brand and it will change the colors of things like checkboxes and radio buttons.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1639311798880/1xzhCW2pD.png)

You can achieve this by just adding the `accent-<color>-<shade>` to the div/form tag.

**File upload button**

You can also customize the file upload buttons to look beautiful like this-


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1639312753634/1DdxR_8sN.png)

You can add styles like you do normally but prefix `file:`. To get the same button as I did you can use this-

```
<input type="file" class="file:bg-emerald-200 file:px-4 file:py-2 file:rounded-full file:border-none file:text-emerald-900 file:text-lg file:font-semibold"/>
```

## Fancy underlines

You can now change the color, thickness, and style of your underlines as well!

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1639313339802/wawXZ-9AH.png)

To use these styles you need to use `decoration-red-600 decoration-wavy` so the styles will look like this-

```
<p class="underline decoration-red-600 decoration-wavy">I have squigly underline</h1>
```


## Conclusion

The team at TailwindCSS has also released some more cool stuff, I just highlighted the things that I found the most exciting. To know more about Tailwind you can check out their  [website ](https://tailwindcss.com/) 😉. See ya next time ✌️

### Useful links

[TailwindCSS](https://tailwindcss.com/)

[Tailwind's video on the release](https://youtu.be/mSC6GwizOag)

[Connect with me](https://links.avneesh.tech/)