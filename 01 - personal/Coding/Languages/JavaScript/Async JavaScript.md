## Callbacks

Think of functions as machines. These machines can do anything you want them to do. They can accept input and return a value. Each machine has a button on it that you can press when you want the machine to run. 

>[!question]- What is a function?
>A function is a block of code that performs a specific task. It can take inputs, process them, and return an output or perform actions without explicitly returning a value. 

```js
function add(x, y) {
	return x + y; 
}

add(2, 3); // 5 - Pressed the button, run the machine.  
```

It doesn’t matter who presses the button, whenever the button is pressed, the machine is going to run. 

```js
function add(x, y) {
  return x + y;
}

const me = add;
const you = add;
const someoneElse = add;

me(2, 3); // 5 - Press the button, run the machine.
you(2, 3); // 5 - Press the button, run the machine.
someoneElse(2, 3); // 5 - Press the button, run the machine.
```

In the code above, we assigned the `add` function to three different variables `me`, `you`, and `someoneElse`. They’re the exact same thing under different names. So when we invoke `me`, `you` or `someoneElse`, it’s as if we’re invoking `add`. 

Now, what if we take `add` machine and pass it to another machine?

```js
function add(x, y) {
  return x + y;
}

function addFive(x, addReference) {
  return addReference(x, 5); // 15 - Press the button, run the machine.
}

addFive(10, add); // 15
```

Instead of “pressing the button” on `add`, we pass `add` as an argument to `addFive`, rename it `addReference`, and then we “press the button” or invoke it. 

>[!note]
>You can pass a string or a number as an argument to a function. You can also pass a reference to a function as an argument. 
>

When you pass a function as an argument, the function you’re passing as an argument is called a **callback** function. The function you’re passing the callback function to is called a **higher order function**

>[!question]-  What is a callback function?
>A callback function is when a function is being referenced as an argument in a higher order function. 

>[!question]- What is a higher order function?
>A function that passes an callback function in its argument

Here is an example with reworded function: 

```js
function add(x, y) {
  return x + y;
}

function higherOrderFunction(x, callback) {
  return callback(x, 5);
}

higherOrderFunction(10, add);
```

This pattern is everywhere. One example is using JavaScript Array methods. 

```js
// array
[1, 2, 3].map((i) => i + 5);

// lodash
_.filter([1, 2, 3, 4], (n) => n % 2 === 0);

// jQuery
$("#btn").on("click", () =>
  console.log("Callbacks are everywhere"),
);
```

There are two popular cases for callbacks. 
1. The first is `.map` and `_.filter`. 
2. The second is jQuery where you want to delay an execution of a function until a particular time. 

Instead of delaying execution of a function until a *particular time*, we can delay execution of a function *until we have the data we need*. 

```js
// updateUI and showError are irrelevant.
// Pretend they do what they sound like.

const id = "tylermcginnis";

$.getJSON({
  url: `https://api.github.com/users/${id}`,
  success: updateUI,
  error: showError,
});
```

We can’t update the UI of our app until we have the user’s data. So what do we do? We give an object and if the request succeeds, we call `success` passing it the user’s data. If it doesn’t we call `error` passing it the error object. 

This is a perfect example of callback for async requests. 

---



