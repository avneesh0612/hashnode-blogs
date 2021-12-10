## How to Build a Todo app with Svelte!

## Demo

%[https://www.loom.com/share/99032ab2d14844cfb0962e0e71ed3891]

## Setup

### Create app

```
npx degit sveltejs/template svelte-todo-app
```

### cd into the folder-

```
cd svelte-todo-app
```

### Install dependencies

```
npm install # npm

yarn install # yarn
```

### Start app

```
npm run dev # npm

yarn dev # yarn
```


### Cleanup
I don't want the default stylings, so I will replace the stylings in `globals.css` with this-

```
* {
  margin: 0;
}

body {
  margin: 0;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
    "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}

code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
    monospace;
}
```

### Building the container

In `App. svelte` let's create the starter code for our app.

```
<script>
</script>

<main class="container">
</main>

<style>
  .container {
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 90vh;
    background: #222e50
      url(https://images.unsplash.com/photo-1579546929518-9e396f3cc809?ixid=MXwxMjA3fDB8MHxzZWFyY2h8N3x8Z3JhZGllbnR8ZW58MHx8MHw%3D&ixlib=rb-1.2.1&auto=format&fit=crop&w=500&q=60)
      no-repeat;
    background-size: cover;
    padding-top: 10vh;
  }
</style>
```


## Building the form to submit Todos

Inside our main container, add a form component with input and button-

```
<main class="container">
  <div>
    <form on:submit|preventDefault={addTodo}>
      <input
        bind:value={newItem}
        type="task"
        class="todos__input"
        placeholder="Enter Todo"
      />
      <button class="todos__button">+</button>
    </form>
  </div>
</main>
```

We need to create the following things now-

- A variable to store the input value

- A `addTodo` function for adding the todos.

So in the script tag add the following-

```
  let newItem = "";
  let todoList = [];

  function addTodo() {
    if (newItem !== "") {
      todoList = [
        ...todoList,
        {
          task: newItem,
          completed: false,
        },
      ];
      newItem = "";
    }

    console.log(todoList);
  }
```

**Styling**

Now let's style our submit button and the input. Inside the styles, tag add this-

```
.todos__input {
    background-color: inherit;
    border: none;
    box-shadow: none;
    text-decoration: none;
    font-size: 1.2rem;
    border-bottom: 1px solid black;
    margin-top: 15px;
    outline: none;
    width: 500px;
  }
  .todos__button {
    background-color: inherit;
    border: none;
    box-shadow: none;
    font-size: 1.2rem;
    cursor: pointer;
  }
```

If we now add an item, it will add the item to the list and console log it.

## Rendering the todos

In React like we have a map function, we do it through `#each` in Svelte

```
 {#each todoList as item, index}
      <div class="todo">
        <span class="todo__text">{item.task}</span>
      </div>
    {/each}
```

**Styling the todos**

```
 .todo {
    display: flex;
    padding: 20px;
    border-radius: 20px;
    box-shadow: 0 0 15px rgb(0 0 0 / 20%);
    background-color: hsla(0, 0%, 100%, 0.2);
    -webkit-backdrop-filter: blur(25px);
    backdrop-filter: blur(25px);
    width: inherit;
    margin-top: 15px;
    font-size: 1.2rem;
    justify-content: space-between;
    align-items: center;
  }
```

This will give the todos a glassmorphic look 🤩

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636888198620/sMgPhSYST.png)


### Adding a header for the todos

The to-do list and the inputs look kinda clumsy, so let's add a header there-

After the form component add this h2 tag-

```
<h2 class="todos__listHeader">Todos</h2>
```

The stylings for this header-

```
 .todos__listHeader {
    text-align: center;
    padding: 20px;
    border-radius: 20px;
    box-shadow: 0 0 15px rgb(0 0 0 / 20%);
    background-color: hsla(0, 0%, 100%, 0.2);
    -webkit-backdrop-filter: blur(25px);
    backdrop-filter: blur(25px);
    margin: 15px 0px 25px 0px;
    font-size: 1.2rem;
  }
```

Now we have got a pretty good separation there.

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1636888409898/VXxzunpnc.png)

## Creating the complete and delete functionality

We are going to use icons for delete and complete, so let's get the icons first.

- Create `Icons. svelte` file in the src directory. Add the following piece of code, for the icons. Feel free to change the icons :) -

```
<script>
  export let name;
  export let width = "1.5rem";
  export let height = "1.5rem";
  export let focusable = false;
  let icons = [
    {
      box: 24,
      name: "check-mark",
      svg: `<svg focusable="false" viewBox="0 0 24 24" aria-hidden="true"><path d="M19.77 4.93l1.4 1.4L8.43 19.07l-5.6-5.6 1.4-1.4 4.2 4.2L19.77 4.93m0-2.83L8.43 13.44l-4.2-4.2L0 13.47l8.43 8.43L24 6.33 19.77 2.1z"></path></svg>`,
    },
    {
      box: 32,
      name: "delete",
      svg: `<svg focusable="false" viewBox="0 0 24 24" aria-hidden="true"><path d="M6 19c0 1.1.9 2 2 2h8c1.1 0 2-.9 2-2V7H6v12zM8 9h8v10H8V9zm7.5-5l-1-1h-5l-1 1H5v2h14V4h-3.5z"></path></svg>`,
    },
  ];
  let displayIcon = icons.find((e) => e.name === name);
</script>

<svg
  class={$$props.class}
  {focusable}
  {width}
  {height}
  viewBox="0 0 {displayIcon.box} {displayIcon.box}">{@html displayIcon.svg}</svg
>
```

**Show the icons**

Inside the todo, I will add a div with two buttons that have the icons as follows-

```
{#each todoList as item, index}
  <div class="todo">
    <span class="todo__text">{item.task}</span>

    <div class="icons">
      <button
        class="icon__button"
        on:click={() => (item.completed = !item.completed)}
      >
        <Icons name="check-mark" class="icon" />
      </button>

      <button class="icon__button" on:click={() => removeFromList(index)}>
        <Icons name="delete" class="icon" />
      </button>
    </div>
  </div>
{/each}
```

Import the icons like this-

```
import Icons from "./Icons.svelte";
```

Creating the delete function-

```
 function removeFromList(index) {
    todoList.splice(index, 1);
    todoList = todoList;
  }
```

**Styling the buttons**

Add the following styles to get a beautiful icon button-

```
 .icon__button {
    background-color: transparent;
    border: none;
    box-shadow: none;
    font-size: 1.2rem;
    cursor: pointer;
    color: rgba(0, 0, 0, 0.54);
  }
  .icon {
    background: rgba(0, 0, 0, 0.54);
  }
```


### Striking the Text
Add this optional class to the item. task span, so if the item is completed then it will add the class-

```
 <span
          class={`todo__text ${item.completed ? "todo__checked--strike" : ""}`}
          >{item.task}</span
        >
```

Now, we need to add the styles to strike it-

```
  .todo__checked--strike {
    text-decoration: line-through;
  }
```

This function takes an argument of `index` and splices the todoList to remove the item.

We have successfully built a to-do app in Svelte! 🥳🎉

To extend your knowledge about svelte check out this video by @[James Q Quick](@jamesqquick) where he will show you how to create a todo app with Sveltekit and tailwind CSS!

%[https://youtu.be/YipaPr4Aex8]


## Useful links

[Github Repository](https://github.com/avneesh0612/Svelte-todo-app-demo)

[Demo](https://svelte-todo-demo.netlify.app/)

[James' channel](https://www.youtube.com/channel/UC-T8W79DN6PBnzomelvqJYw)

[Connect with Me](https://links.avneesh.tech/) 