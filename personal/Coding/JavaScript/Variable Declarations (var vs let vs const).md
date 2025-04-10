- [[#Declaration vs. Initialization:|Declaration vs. Initialization:]]
- [[#Scope (Where variables are accessible):|Scope (Where variables are accessible):]]
- [[#Hoisting:|Hoisting:]]
- [[#Key Differences & When to Use:|Key Differences & When to Use:]]
	- [[#Key Differences & When to Use:#General Recommendations:|General Recommendations:]]
## Declaration vs. Initialization:
- **Declaration**: Introducing a variable name (e.g. `var myVar;`). It gets the default value `undefined`

```js
var declaration;
```

- **Initialization**: Assigning a value to a variable for the first time (e.g., `myVar = 'Hello';).

```js
var declaration

console.log(declaration) // undefined

declaration = 'This is an initialization'
```
## Scope (Where variables are accessible):

> "If the variable statement occurs inside a `FunctionDeclaration`, the variables are defined with function-local scope in that function.".

- **Function Scope** (`var`): Variables declared with `var` are accessible anywhere within the function they are created in, even outside of loops or `if` blocks within that function.

- **Block Scope** (`let`, `const`): Variables declared with `let` or `const` are only accessible within the specific block (code surrounded by `{ }`, like `if` statements or `for` loops) they are created in. 

```js
function getDate() {
  var date = new Date();

  function formatDate() {
    return date.toDateString().slice(4); // ✅
  }

  return formatDate();
}

getDate();
console.log(date); // ❌ Reference Error
```
## Hoisting:
- `var`: Declarations are moved to the top of their function scope by JavaScript *before* the code runs and are initialized with `undefined`. This means you can access a `var` variable before its actual line in the code, but its value will be `undefined`. 

- `let` & `const`: Declarations are also moved to the top of their block scope, but they are *not* initialized. Trying to access them before their declaration line results in a `ReferenceError` (this is sometimes called the "Temporal Dead Zone"). 

```js
function discountPrices(prices, discount) {
  var discounted = undefined;
  var i = undefined;
  var discountedPrice = undefined;
  var finalPrice = undefined;

  discounted = [];
  for (i = 0; i < prices.length; i++) {
    discountedPrice = prices[i] * (1 - discount);
    finalPrice = Math.round(discountedPrice * 100) / 100;
    discounted.push(finalPrice);
  }

  console.log(i); // 3
  console.log(discountedPrice); // 150
  console.log(finalPrice); // 150

  return discounted;
}
```

Comparing `var`, `let`, and `const`:

| Feature                 | `var`                         | `let`                                       | `const`                                     |
| ----------------------- | ----------------------------- | ------------------------------------------- | ------------------------------------------- |
| **Scope**               | Function                      | Block                                       | Block                                       |
| **Hoisting**            | Hoisted & `undefined` default | Hoisted, no default (`ReferenceError`, TDZ) | Hoisted, no default (`ReferenceError`, TDZ) |
| **Reassignment**        | Yes                           | Yes                                         | No (must be initialized)                    |
| Redeclaration           | Yes (in same scope)           | No (in same scope)                          | No (in same scope)                          |
| **Use Before Declared** | Returns `undefined`           | `ReferenceError`                            | `ReferenceError`                            |
|                         |                               |                                             |                                             |

## Key Differences & When to Use:
- `var` vs. `let`: The main difference is scope (`function` vs. `block`). `let` helps prevent errors common with `var` where variables "leak" out of loops or conditionals. Also, `let` gives an error if accessed before declaration, which is often safer than `var` returning `undefined`.

- `let` vs. `const`: Both are block-scoped and throw errors if accessed before declaration. The *only* difference is reassignment. Use `const` when you know the variable's value shouldn't change after it's set. Use `let` when you *need* to reassign the variable later.

- `const` and Objects/Arrays: Declaring an object or array with `const` means the variable cannot be reassigned to a *different* object or array. However, you *can* still change the properties of the object or the elements of the array. 

### General Recommendations:
1. Default to `const`: Use `const` whenever possible. This signals that the variable binding shouldn't change
2. Use `let` when reassignment is needed: If you know the variable's value will need to change (e.g., loop counters, value swaps), use `let`
3. Avoid `var`: In modern JavaScript (ES6+), there's generally little reason to use `var`. Using `let` and `const` leads to cleaner, less error-prone code due to block scoping and the TDZ

