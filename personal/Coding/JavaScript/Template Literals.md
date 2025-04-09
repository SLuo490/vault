Template Literals makes string concatenation significantly easier when given variables and creating a sentence using them. 

For example this string concatenation uses three variables and make a greeting. 

```js
function makeGreeting(name, email, id) {
  return (
    "Hello, " +
    name +
    ". We've emailed you at " +
    email +
    '. Your user id is "' +
    id +
    '".'
  );
}
```

By using template literals, we can simplify the code above to this

```js
function makeGreeting(name, email, id) {
  return `Hello, ${name}. We've emailed you at ${email}. Your user id is "${id}".`;
}
```

Now instead of having a `makeGreet` function, say we wanted a `makeGreetingTemplate` function that returned us an HTML string that we can throw at the DOM. Without template literals, we would having this.

```js
function makeGreetingTemplate(name, email, id) {
  return (
    "<div>" +
    "<h1>Hello, " +
    name +
    ".</h1>" +
    "<p>We've emailed you at " +
    email +
    ". " +
    'Your user id is "' +
    id +
    '".</p>' +
    "</div>"
  );
}
```

But with template strings we can also create multi-line strings. 

```js
function makeGreetingTemplate(name, email, id) {
  return `
    <div>
      <h1>Hello, ${name}</h1>
      <p>
        We've email you at ${email}.
        Your user id is "${id}".
      </p>
    </div>
  `;
}
```