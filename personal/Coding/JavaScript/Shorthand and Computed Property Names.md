## Shorthand Properties
With shorthand properties, whenever you have a variable which is the **same name** as a property on an object, you can omit the property name when constructing the object.

```js
function formatMessage (name, id, avatar) {
  return {
    name: name,
    id: id,
    avatar: avatar,
    timestamp: Date.now()
  }
}
```

to this

```js
function formatMessage (name, id, avatar) {
  return {
    name,
    id,
    avatar,
    timestamp: Date.now()
  }
}
```

### Shorthand Method Names
What if one of the properties was a function?

A function that is a property on an object is called a method. With the ES6's Shorthand Method Names, you can omit the `function` keyword completely.

```js
function formatMessage (name, id, avatar) {
  return {
    name,
    id,
    avatar,
    timestamp: Date.now(),
    save: function () {
      // save message
    }
  }
}
```

to this

```js
function formatMessage (name, id, avatar) {
  return {
    name,
    id,
    avatar,
    timestamp: Date.now(),
    save () {
      //save message
    }
  }
}
```

## Computed Property Names

ES6's Computed Property Names feature allows you to have an expression be computed as a property name on an object. 

For example, you want to create a function that takes in two arguments (`key`, and `value`) and returned an object using those arguments. 

Before the Computed Property Names, we would need to create the object first, then use bracket notation to assign that property to the value.

```js
function objectify(key, value) {
  let obj = {};
  obj[key] = value;
  return obj;
}

objectify("name", "Tyler"); // { name: 'Tyler' }
```

But with Computed Property Names, we can use object literal notation to assign the expressions as property on the object without having to create it first. 

```js
function objectify(key, value) {
  return {
    [key]: value,
  };
}

objectify("name", "Tyler"); // { name: 'Tyler' }
```

Where `key` can be any expression as long as it's wrapped in brackets, `[]`. 

