- [[#Regular Function|Regular Function]]
- [[#Arrow Function|Arrow Function]]
- [[#Benefits of Arrow Function|Benefits of Arrow Function]]
	- [[#Benefits of Arrow Function#Implicit Returns|Implicit Returns]]
	- [[#Benefits of Arrow Function#[[this]]|[[this]]]]
- [[#Nice to knows|Nice to knows]]


There are two main benefits with arrow functions over regular functions. 
1. Arrow functions are brief and to the point
2. Make managing the `this` keyword easier

## Regular Function
Here is a basic function declaration and a function expression:

```js
// fn declaration
function add(x, y) {
  return x + y;
}

// fn expression
var add = function (x, y) {
  return x + y;
};
```

## Arrow Function
Now to change the function expression to an arrow function:

```js
var add = function (x, y) {
  return x + y;
};

var add = (x, y) => {
  return x + y;
};
```

## Benefits of Arrow Function
Arrow function thrives when using anonymous functions. For example, using `.map`.

```js
users.map(function() {});

users.map(() => {}); 
```

Another example, let say we had a `getTweets` function that took in a user id and, after hitting a poorly designed API, returned us all of the user's Tweets with over 50 stars and retweets. 

Using promise chaining, the function may look like this,

```js
function getTweets(uid) {
  return fetch("//api.users.com/" + uid)
    .then(function (response) {
      return response.json();
    })
    .then(function (response) {
      return response.data;
    })
    .then(function (tweets) {
      return tweets.filter(function (tweet) {
        return tweet.stars > 50;
      });
    })
    .then(function (tweets) {
      return tweets.filter(function (tweet) {
        return tweet.rts > 50;
      });
    });
}
```

Now lets look at how arrow function can improve the readability of this code

```js
function getTweets(uid) {
  return fetch("//api.users.com/" + uid)
    .then((response) => {
      return response.json();
    })
    .then((response) => {
      return response.data;
    })
    .then((tweets) => {
      return tweets.filter((tweet) => {
        return tweet.stars > 50;
      });
    })
    .then((tweets) => {
      return tweets.filter((tweet) => {
        return tweet.rts > 50;
      });
    });
}
```

We basically removed the `function` and added the "=>" function. 

### Implicit Returns
Arrow function also has a benefit which is implicit returns. 

With arrow functions, if your function has a "concise body" of a one line function, then you can omit the "return" keyword and the value will be returned automatically (or implicitly). 

So the `add` function would look like this,

```js
var add = function (x, y) {
  return x + y;
};

var add = (x, y) => x + y;
```

and the `getTweets` example can be updated to look like this,

```js
function getTweets(uid) {
  return fetch("//api.users.com/" + uid)
    .then((response) => response.json())
    .then((response) => response.data)
    .then((tweets) => tweets.filter((tweet) => tweet.stars > 50))
    .then((tweets) => tweets.filter((tweet) => tweet.rts > 50));
}
```

Now the code is much easier to write and much easier to read. 

Another change we can make is that if the arrow function only has one parameter, we can omit the `()` around it. 

```js
function getTweets(uid) {
  return fetch("//api.users.com/" + uid)
    .then(response => response.json())
    .then(response => response.data)
    .then(tweets => tweets.filter((tweet) => tweet.stars > 50))
    .then(tweets => tweets.filter((tweet) => tweet.rts > 50));
}
```

### [[this]]

Let's look at a typical React code.

```js
class Popular extends React.Component {
  constructor(props) {
    super();
    this.state = {
      repos: null,
    };

    this.updateLanguage = this.updateLanguage.bind(this);
  }
  componentDidMount() {
    this.updateLanguage("javascript");
  }
  updateLanguage(lang) {
    api.fetchPopularRepos(lang).then(function (repos) {
      this.setState(function () {
        return {
          repos: repos,
        };
      });
    });
  }
  render() {
    // Stuff
  }
}
```

When the component mounts, it's making an API request (to the Github API) to fetch JavaScript's most popular repositories. When it gets the repositories, it takes them and updates the component's local state. But there is an bug in the code. 

The error the code will throw is "cannot read setState of undefined". The reasoning of this error can be seen in [[this]] JavaScript note. A typical ES5 solution uses `.bind`. 

```js
class Popular extends React.Component {
  constructor(props) {
    super();
    this.state = {
      repos: null,
    };

    this.updateLanguage = this.updateLanguage.bind(this);
  }
  componentDidMount() {
    this.updateLanguage("javascript");
  }
  updateLanguage(lang) {
    api.fetchPopularRepos(lang).then(
      function (repos) {
        this.setState(function () {
          return {
            repos: repos,
          };
        });
      }.bind(this), // here
    );
  }
  render() {
    // Stuff
  }
}
```

Arrow functions **don't create their own context**. Typically the `this` keyword just works without you having to worry about what context a specific function is going to be invoked. So by using arrow functions in the `updateLanguage` method, we don't have to worry about `this` which means we don't have to call `.bind` anymore. 

```js
updateLanguage(lang) {
  api.fetchPopularRepos(lang)
    .then((repos) => {
      this.setState(() => {
        return {
          repos: repos
        }
      });
    });
}
```

## Nice to knows

If we want to implicitly return the object inside the setState callback, how would we do that? The first intuition would be to remove the return statement and just return an object. 

```js
api.fetchPopularRepos(lang).then((repos) => {
  this.setState(() => {
    repos: repos;
  });
});
```

The problem with this is that the syntax is the exact same as creating a function body. JavaScript can't tell the difference between when you want to create a function body and when you want to return an object so it'll throw an error. 

To fix this, we can wrap the object inside of `()`.

```js
api.fetchPopularRepos(lang).then((repos) => {
  this.setState(() => ({
    repos: repos;
  }));
});
```

Now the arrow function will implicitly return an object. 

---

We can also use ES6's [[Shorthand and Computed Property Names]] feature to get rid of the `repos:repos` and use Arrow function's implicit return to shorten it. 

```js
api
  .fetchPopularRepos(lang)
  .then((repos) => this.setState(() => repos));
```

---

Say we wanted to examine the previous state of the component inside of setState by logging it. 

```js
this.setState((nextState) => ({
  repos: repos,
}));
```

The obvious move would be to change your implicit return to an explicit return, create a function body, then log inside of that body. 

```js
this.setState((nextState) => {
  console.log(nextState);
  return {
    repos: repos,
  };
});
```

A better way would be to use the `||` operator. Instead of messing with all your code, you can do this

```js
this.setState(
  (nextState) =>
    console.log(nextState) || {
      repos: repos,
    },
);
```