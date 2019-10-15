# Promises
​
## Prior Knowledge
​
- able to make asyncronous code reusable using callback functions
- understand the MVC pattern
​
## Learning Objectives
​
- use promises
- promisifying callbacks
- use 3rd party apis as a service to manipulate and analyse data
- composing multiple 3rd party api requests
- reinforce the MVC pattern
​
​
### Promises
​
The Promise object is used for asynchronous computations. A Promise represents a placeholder for an asynchronous operation that hasn't completed yet, but is expected in the future. [MDN - Promise Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
​
Instead of immediately returning the final value, the asynchronous method returns a promise to supply the value at some point in the future.
​
Promises have 3 possible states:
​
- **pending**: initial state, neither fulfilled nor rejected.
- **fulfilled**: meaning that the operation completed successfully.
- **rejected**: meaning that the operation failed.
​
Once a promise is fulfilled or rejected, its state cannot change.
​
When fulfilled/resolved, the promise's `.then()` method is called with the result of the asynchronous task.
When rejected, the `.catch()` method is called with the error. [Using promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)
​
​
## Benefits of using Promises
​
Developers will have varying opinions on the pros and cons of callbacks vs. promises. Here are some of the arguments for using promises:
​
- Improved readability
- Reduced nesting of code (no callback hell!)
- Error propagation - Basically, a promise chain stops if there is an exception, looking down the chain for catch handlers instead
​
## Creating a Promise
​
Although many packages that you come across will already use Promises, there may be times where you want to refactor asynchronous code that uses callback functions to use _Promises_ instead.
​
Consider the following `fs.readFile` example:
​
```js
const fs = require('fs');
​
const readFileUTF8 = (path, cb) => {
  fs.readFile(path, 'utf8', (err, data) => {
    if (err) cb(err);
    else cb(null, data);
  });
};
​
readFileUTF8('./cats', (err, cats) => {
  if (err) console.log(err);
  else console.log(cats);
});
```
​
We could _**promisify**_ the function so that it will work in a similar way to the previous axios request.
​
In order to create a function that will return a promise, we must first understand how to create a new `Promise`.
​
A `Promise` object is created by using the `new` keyword and its constructor (`Promise`). The constructor takes as its argument a function, called the **"executor function"**.
​
The "executor function" take two functions as parameters. The first of these functions (`resolve`) is called when the asynchronous task completes successfully and returns the results of the task as a value. The second (`reject`) is called when the task fails, and returns the reason for failure (typically an error object).
​
```js
// ...
const returnsPromise = () => {
  return new Promise((resolve, reject) => {
    someAsyncFunction((err, data) => {
      if (err) reject(err);
      else resolve(data);
    });
  });
};
// ...
returnsPromise()
  .then((data) => {
    // do stuff with data
  })
  .catch((err) => {
    // handle error
  });
```
​
So for the `fs.readFile` example, we could also promisify it in this way:
​
```js
const fs = require('fs');
​
const readFileUTF8Promise = (path) => {
  return new Promise((resolve, reject) => {
    fs.readFile(path, 'utf8', (err, data) => {
      if (err) reject(err);
      else resolve(data);
    });
  });
};
​
readFileUTF8Promise('./cats')
  .then((cats) => console.log(cats))
  .catch((err) => console.log(err));
```