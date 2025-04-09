## What is Destructuring?
- Destructuring is an ES6 feature that provides a concise way to **extract multiple values** from **objects** and **arrays** and assign them directly into variables

- It simplifies code by reducing the need to access properties or elements one by one

### Why use it?
- **Before**: You had to extract values individually:

```js
const user = {};
user.name = "Tyler McGinnis";
user.handle = "@tylermcginnis";
user.location = "Eden, Utah";

const name = user.name;
const handle = user.handle;
```

- **With Destructuring**: Extract multiple values in one line:

```js
// For Objects
const { name, handle, location } = user;

// For Arrays
const [name, handle, location] = userArray;
```

This makes code shorter and often more readable.

## Key Features & Uses:

### **Object Destructuring**:
Extracts properties based on their **name**

**Renaming**: You can assign the extracted property to a variable with a different name:

```js
const user = {
  n: "Tyler McGinnis",
  h: "@tylermcginnis",
};

const { n: name, h: handle } = user; 
// Extracts 'n' into 'name', 'h' into 'handle'
```

**Function Results**: Easily destructure the object returned by a function:

```js
const { name, handle } = getUserFunction();
```

### **Array Destructuring:**

Extracts elements based on their **position** (index)

Useful when the order/position of elements is significant (e.g., parsing CSV data, handling results from `Promise.all`).

**Function Results**: Easily destructure the array returned by a function:

```js
const cvs = "1997,Ford,F350,MustSell!";
const [year, make, model, description] = csv.split(",");
```

### **Destructuring in Function Parameters:** (Very powerful!)

**Object Parameters:** Instead of passing many arguments in a specific order, pass a single object and destructure it in the function definition.

```js
// Old way (order matters, null for unused)
// fetchRepos('JS', 100, null, date, null);

// New way (object argument)
fetchRepos({ language: 'JS', minStars: 100, createdBefore: date });

// Function definition using destructuring
function fetchRepos({ language, minStars, createdBefore }) {
  // ... use language, minStars, createdBefore directly
}
```

**Benefits**:
- **Named Parameters**: Arguments are self-documented
- **Order Doesn't Matter**: No need to remember argument positions
- **Optional Parameters**: Easily skip properties in the object you pass

**Default Values**: Set default values directly in the parameter destructuring:

```js
function fetchRepos({ language = 'All', minStars = 0 }) {
  // ... if language/minStars aren't provided, they get default values
}
```

**Array Parameters:** You can also destructure array arguments directly, useful for functions like `.then()` callbacks in `Promise.all`:

```js
Promise.all([promise1, promise2])
  .then( ([result1, result2]) => { // Destructure the results array here
    // ... use result1, result2
  });
```

## Rest Syntax (`...`)
The syntax `...` collects the remaining properties (objects) or elements (arrays) into a new structure

**Object Rest**: Gathers all *other* properties into a new object. Often used to separate specific properties from the rest (e.g., in React props).

```js
const user = { name: 'Bob', age: 30, isAdmin: false, theme: 'dark' };
const { name, age, ...otherProps } = user;

console.log(name);       // Bob
console.log(age);        // 30
console.log(otherProps); // { isAdmin: false, theme: 'dark' }
```

**Array Rest:** Gather all remaining elements into a new array. Very common for handling variable numbers of function arguments. 

```js
const numbers = [10, 20, 30, 40, 50];
const [firstNum, secondNum, ...restOfNumbers] = numbers;

console.log(firstNum);       // 10
console.log(secondNum);      // 20
console.log(restOfNumbers); // [30, 40, 50]
```