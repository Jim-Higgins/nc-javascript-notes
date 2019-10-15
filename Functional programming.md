# Functional programming
​
## Prior Knowledge
​
- Understand the anatomy of a function
​
## Learning objectives
​
- Understand the basic concepts of functional programming
- Understand and identify pure functions
- Know how to write tests to ensure functions are not having side effects
​
Consider the examples below:
​
```js
function printDate() {
  return Date.now();
}
printDate() // returns 1568907696733
printDate() // returns 1568907711505
​
​
let a = 3;
function findSum(b) {
  return a + b;
}
​
findSum(10); // return 13
​
a = 32;
findSum(10) // returns 42;
```
​
In the function `printDate` its return value is not dependent on the input. We can pass any number of arguments into this function but it will not affect the eventual output. It will continue to return the current time in miliseconds regardless of what inputs are passed to the function.
​
The function `findSum` totals the values of `a` and `b`: however, b is a locally scoped variable. In other words, the computation of the sum inside this function depends on upon its input **and** variables or state that exist outside of it.
​
Consider now another example:
​
```js 
​
const person = {
  name: 'Anat',
  age: 23
};
​
function updateAge(person, amount) {
  person.age += amount;
  return person;
}
​
updateAge('Anat',10)
/* returns
{
  name: 'Anat',
  age: 33
};
*/
```
​
In the above example, the return value is dependent on the input. However, the first argument passed to the function `person` is held on reference. Inside the function the original person object is being changed or **mutated**. This act of changing something external to a function is known as a _side effect_. We could say that any external re-assignment of a variable or structure outside of the function is a side effect. 
​
Consider the example below:
​
```js
function doubleList(list) {
  const doubledList = [];
  for (let i = 0; i < list.length; i++) {
    doubledList.push(list[i] * 2);
  }
  return doubledList;
}
​
const doubles = doubleList([1,2,3]);
```
​
The function `doubleList` in its implementation depends on an alteration of state: in this case, we consider the variable `i` as having a state that is being altered. In other words, state in this example is the variable and `i`, and as the iteration is started the value of this variable is altered. Essentially the function is performing some kind of 
​
All of the previous examples 
​
​
A paradigm is a way of doing something. In software terms, a paradigm refers to the ways in which we write our code. The functional programming paradigm has various features but one of the things that is prioritised is the use of pure functions. 
​
## Pure functions
​
**Pure functions** are functions that:
- given the same inputs, always returns the same output 
- have no side effects
​
### Predictability
​
The advantage of writing **pure functions** is that they are predictable or deterministic. Their return value is dependent solely on the inputs so we can be absolutely sure what our function will return when invoked with some particular arguments. 
​
### Safety
​
Pure functions also give us more safety in our code as they should have no **side effects**. This means we can be certain that state outside our code has not been modified or mutated. Another central idea to functional programming is the idea of **immutability** - the idea that structures, this could be arrays or objects, are not mutated by functions. The implementation of pure functions in our code should prevent side effects and help to ensure that state in our code is **immutable**.
​
The functional programming paradigm is about writing code that emphasies the use of:
​
- pure functions
- avoidance of shared state, mutable data, side effects
- declarative rather than an imperative approach
- higher order patterns
​
## Testing for Purity
​
In practice, it is impossible to devise a set of tests that prove a function is completely pure. However, we can use some tests to ensure that the inputs to our function have not been mutated and that we are returning new references. These tests can offer some security that we have not modified our inputs and hence prevent the introduction of undesirable side effects.
​
### Returning a new array or object
​
Suppose a function `doubleNums` takes an array and needs to return an array with all the values doubled. As a condition of the function being pure, it must have to return a new array. If it returned a reference to the original array this would imply we have modified the contents of the original array, meaning that the inital input was not **immutable**. We must therefore ensure that the function returns a new array. 
```js
// ./index.test.js
​
test('returns a new array',() => {
  const input = [42,100,5];
  const output = doubleNums(input);
  expect(input).to.notBe(output);
});
```
​
In the above test case, we are trying to prove that the return value of the function `doubleNums` is not pointing to the `input`. However, this test is not sufficient in guaranteeing that the `input` has not been mutated. We could return a new array but also mutate the original input.
​
### Checking for mutation
​
```js
function doubleNums (input) {
  
  const result = [];
  for (let i = 0; i < input.length; i++) {
    const currentVal = input.shift();
    result.push(currentVal);
  }
  return result;
}
```
This function returns a new array but is also mutating the original input. We can write a test to prove that the input is not being mutated:
​
```js
test('input is not mutated',() => {
  const input = [42, 100, 5];
  doubleNums(input);
  expect(input).toEqual([42,100,5]);
});
```
In this test, we are not interested in making an assertion with the return value of the function `doubleNums` instead we are interested in invoking the function and then proving that the initial input is still the same as it was after the function's invocation.
​
## Higher order functions
​
In JavaScript, functions are regarded as _first class citizens_ meaning they can be passed around like other data types. This means we can pass functions, like other variables, as arguments into other functions. In a similar way functions can also return other functions in the same way they can return numbers, strings and other primitive values. This concept leads us to the definition of **higher order functions**:
​
A **higher order function** is a function that:
- can accept functions as arguments
- return functions
​
Higher order functions are a hugely important part of the functional programming paradigm, they allow us to generalise functionality and to write code that is more _declarative_ as opposed to code that is more _imperative_ in nature.
​
Consider the two functions below which create new arrays.
​
```js
function copyArrayAndSquare(list) {
  const output = [];
​
  for (let i = 0; i < list.length; i++) {
    output.push(list[i] ** 2);
  }
  return output;
}
​
function copyArrayAndTriple(list) {
  const output = [];
​
  for (let i = 0; i < list.length; i++) {
    output.push(list[i] * 3);
  }
  return output;
}
​
const nums = [1, 2, 3, 4, 5, 6, 7];
const squaredNums = copyArrayAndSquare(nums); // [1, 4, 9, 16, 25, 36, 49];
const tripledNums = copyArrayAndTriple(nums); // [3, 6, 9, 12, 15, 18, 21];
```
​
In the two examples above, the functions are implemented in an imperative style of code.
​
For example, in `copyArrayAndTriple`, the functions is:
  1. declaring and initialising a new variable as an empty array
  2. setting up a `for` loop
  3. describing what task should be performed on each iteration (tripling and pushing)
  4. returning some output
​
This is an example of what is meant by imperative code, where the algorithm or implementation describes precisely _how_ something is to be done.
​
The second issue arising from these examples is that their procedures are almost identical. The only difference between `copyArrayAndTriple` and `copyArrayAndSquare` is what they are doing on each iteration.
​
#### Generalising functionality
​
We can introduce a new function `copyArrayAndDoStuff` that takes an additional argument - a function. A function that takes another function as an argument is a **higher-order function**. This means we can pass in a set of instructions that does some operation and returns out a value that we can then use to build up our output array.
​
```js
function copyArrayAndDoStuff(list, instructions) {
  const output = [];
  // we still create a variable that hold the new values
​
  for (let i = 0; i < list.length; i++) {
    // here we call instructions to do something to the current
    // array element and then push the returned value from instructions to our
    // output array
    output.push(instructions(list[i]));
  }
  return output;
}
```
Now we can create a function like `tripleNumThenAdd5` and pass it to our `copyArrayAndDoStuff` which will create the new array and build up the output with the for loop. This function `copyArrayAndDoStuff` emulates the behaviour of the native JavaScript `map`. It solves the problem of us duplication the steps of setting up an array and iterating through all its elements: instead we focus on passing a function that will represent the action to be taken at each stage of iteration. 
​
```js
function squareList(list) {
  return list.map(num => num ** 2);
}
```
​
The example above will give the same output as the function `copyArrayAndSquare`. However this style of coding is _declarative_, we are saying what we want instead of stating exactly what needs to be done. The implementation of `map` is not exposed to us instead it is like we are providing a function that squares a single number and trusting that `map` provides us with the correct output. It could be the case that `map` has an underlying implementation that is imperative: however, we are concerned with the tools we are interacting with instead of examining underlying native implementations. We could say at the surface level we have a function that is working declaratively put under the hood `map` is implemented in an imperative way.