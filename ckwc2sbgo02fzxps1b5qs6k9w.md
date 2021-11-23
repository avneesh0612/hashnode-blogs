## 5 JavaScript Array methods you need to know!

While building applications you would have noticed yourself using Array's a lot. So let's take a look at the 5 Array methods.

To show you all the methods with examples, I am going to create a simple Array with some fruits and their prices-

```
const fruits = [
   {
    name: "Mango",
    price: 3,
  },
  {
    name: "Apple",
    price: 5,
  },
  {
    name: "Banana",
    price: 10,
  },
];
```

### 1. Map
A map function is similar to loop and if you are a React developer you probably have seen this function be used quite a lot. So let's understand this properly with an example-

```
function getFruitNames() {
  return fruits.map((fruit) => fruit.name);
}

console.log(getFruitNames());
```

The `getFruitNames` maps through the fruits Array and returns the name of each fruit. So if we run the function it will return an Array with all the fruit names-

```
['Mango', 'Apple', 'Banana']
```

### 2. Filter
If you want to filter some products out based on some conditions like getting the fruits that cost more than 4 then we will use the filter function to do so. This is how you use a filter function-

```
function getCostlyFruits() {
  return fruits.filter((fruit) => fruit.price > 4);
}

console.log(getCostlyFruits());
```

This function returns-

```
[
  { name: "Apple", quantity: 5 },
  { name: "Banana", quantity: 10 },
];
```

### 3. Reduce
Reduce lets you add up a bunch of numbers inside an Array, in a very easy way.

![The Math GIF](https://media0.giphy.com/media/DHqth0hVQoIzS/200.gif)

Let's see it in action-

```
function getTotalCost() {
  return fruits.reduce((acc, fruit) => acc + fruit.price, 0);
}

console.log(getTotalCost());
```

So the first parameter is the number formed by adding the previous numbers and the second parameter is the amount that will get added to the first value. The 0 that you see at the end can be replaced with the number you want the count to start with.

This function outputs a number, in this case- `18`

### 4. Find
If you want to find fruit based on its price or by its name, then this is the method you are looking for. 

```
function getFruitByName(name) {
  return fruits.find((fruit) => fruit.name === name);
}

console.log(getFruitByName("Mango"));
```

This will return the Mango object when run-

```
{ name: 'Mango', price: 3 }
```


### 5. Includes

This method lets you check if there is an item in that Array. Suppose I want to check if banana is present in the fruits Array. If it is present then it will return true, otherwise false. You can't check items inside an object in the Array, so I am going to use the Array with the names from our `getFruitNames` function-

```
const fruitNames = getFruitNames();

console.log(fruitNames.includes("Orange"));
```

This will return `false` as Orange isn't inside the fruits Array, but if we try Apple then it would return `true`-

```
const fruitNames = getFruitNames();

console.log(fruitNames.includes("Apple"));
```


Hope you found this useful. If you have any questions, drop them down in the comments. Until next time Peace ✌️

**Useful Links-**

[CodePen](https://codepen.io/avneesh0612-the-bold/pen/ExvzLgB)

[Connect with me](https://avneesh-links.vercel.app/)