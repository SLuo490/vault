Default parameters are used to set a default value for an argument in an function.

Before default parameters were introduced, one way to set default values is utilizing logical operators (`||`). 

```js
function calculatePayment(price, salesTax, discount) {
  salesTax = salesTax || 0.05;
  discount = discount || 0;

  // math
}
```

The `||` operator can be thought as an `if` statement that checks for falsy values. 

```js
function calculatePayment(price, salesTax, discount) {
  if (!salesTax) {
    salesTax = 0.05;
  }

  if (!discount) {
    discount = 0;
  }

  // math
}
```

However, using the `||` operator approach has some downsides. One downside is that what happen if `salesTax` is `0`? With the current implementation, that would be impossible since `0` is classified as a falsy value so our `if (!salesTax)` would always evaluate to `true` setting the `salesTax` to our default value of `0.05`. 

To fix this, you can check for `undefined` rather than `falsy`. 

```js
function calculatePayment(price, salesTax, discount) {
  salesTax = typeof salesTax === "undefined" ? 0.05 : salesTax;
  discount = typeof discount === "undefined" ? 0 : discount;

  // math
}
```

But there is a better approach which is ES6's "Default Parameters". 

Default parameters allow you to set default values for any parameters that are `undefined` when a function is invoked. 

```js
function calculatePayment(price, salesTax = 0.05, discount = 0) {
  // math
}
```

Now, `salesTax` or `discount` are `undefined` when `calculatePayment` is invoked, they'll be set to their default values of `0.05` and `0`. 

## Required Arguments
One trick you can do with Default Parameters is to throw an error if a function is invoked without a required argument. 

For example, what if we wanted `calculatePayment` to throw an error if the `price` wasn't specified when it was invoked?

To do this, create a function that will throw the error. 

```js
function isRequired(name) {
  throw new Error(`${name} is required`);
}
```

Next, using Default Parameters, assign the required parameter to the invocation of `isRequired`

```js
function calculatePayment (
  price = isRequired('price'),
  salesTax = 0.05,
  discount = 0
) {

  // math
}
```