# Testing
​
## Prior Knowledge
​
- How to export and require something from a JS file in NodeJS
​
## Learning Objectives
​
- How to write unit tests using `jest`
- How to install modules with `npm`
- Understand how to break down a function into distinct test cases
- Understand the difference between a `dependency` and a `devDependency`
​
## Defining tests
​
When writing source code for an application we are implementing or building something that produces certain behaviours - our outputs. At the most basic level this could be a function that returns a particular value when given some inputs. If we know what desired behaviour or outcome our code or application is supposed to produce, the next natural thing to consider would be "how do we know our source code is producing this behaviour?"
​
We could just execute and inspect the result to see if the code is working in the way we expect - however, this solution is not scalable. If a later feature is added then how can we prove that the previous features we had before are still working? In the event that the implementation needs to be modified in some way, then how do we prove that the modified code is still working?
​
We also need to be aware of whether or not it is easy for multiple people (especially those who haven't written the source code) to prove that the code is still behaving as intended. Instead of using inspection to check if our code is working properly, we need a more robust mechanism for checking if our code is working in the way it should. We can write **test** in order to demonstrate if our code is producing some kind of desired behaviour.
​
CUT or SUT are acronyms meaning code under test or system under test - they refer to the source code that we are actually testing at a particular moment. A **test** can be defined as a piece of code that invokes the CUT and then checks that some desired behaviour has occurred. If our CUT or (SUT) fails to produce this desired behaviour then the test will throw an error, informing us that the test has failed.
​
## Benefits of tests
​
### Finding bugs
​
If our CUT is producing some kind of undesirable behaviour then we refer to this is a **bug**. Tests should therefore expedite the process of finding and eliminating bugs from our CUT. In short, we have expectations that code should work in a particular way and tests should allow us to prove this easily.
​
### Maintainable code
​
A suite of tests should also enable us to write more **maintainable code** - code that takes less time to update and add features to in the future. If we decide to add a new piece of behaviour then we could use previously written tests to see if our previous behaviours haven't broken after the introduction of a new feature. In other words, tests should also help us to show that our code continues to work after inevitable revisions and additions over time.
​
### Documentation
​
If tests describe the way our code _should_ behave then they can also serve as a useful supplement but not substitute for documentation. This will especially be the case if the tests give a clear and accurate description of the specific behaviour that is being tested for.
​
### Continuous integration
​
## Types of tests
​
• Unit Testing - testing a small unit of code (typically a function) for specific inputs /outputs. These are quick, small and are generally written to develop the code.
​
• Integration Testing - testing that multiple (individually tested) units work together. We know all the units work, but we need to make sure they are connected in the right way. Slower and more complicated than unit testing
​
• End to End Testing - testing the application flow as a complete system. Typically used to test _critical paths_ such as user login or payment
​
## Unit tests
​
We need to ensure that our unit tests have the following properties to ensure they are effective:
​
### - Easy to write
​
Unit tests should be easy to write and complex unit tests may indicate that the CUT is in fact not a unit.
​
### - Automated and repeatable
​
If we have large number of unit tests then we want to be able to run them at the push of a single button or perhaps with a single commmand.
​
### - Quick
​
As we expect to be writing more unit tests then any other type of test when building our application, it is critical that they run quickly so we can see where the code breaks and then fix the issue. The quicker that a test suite runs means it is more likely to be used a lot 
​
### - Easy to interpret
​
If an error occurs then it should be easy to detect where the error is coming from in the CUT.
​
​
# Test Driven Development (TDD)
​
When writing good quality code we need to make sure our functions are fully tested. The process we will use to achieve this is called **test-driven-development**.
​
When naming our test files they have all been using the `.spec.js` extension. This is short for specification as the only behaviour we can rely on is what is specified in the tests.
​
First we need to decide on what behaviour we would like to test. We should start with the simplest behaviour first. Once that is working we can add another piece of functionality and test that independently. This allows us to build up to an eventual solution so that we don't have to solve the whole problem at once.
​
## Red - green - refactor cycle
​
This process is referred to as the red-green-refactor cycle
​
1. We first decide what functionality we want to implement.
2. Write a test for it, watch it fail (red)
3. Write the minimum amount of code that will make that test pass (green)
4. Refactor code if necessary
5. Repeat.
​
We test the behaviour of our function, not the implementation. Testing a particular implementation is brittle because if you change the way you do something, your tests will need to change too.
​
For example, we are not interested in whether a function uses a for loop or a while loop, so long as it produces the required result. As such our tests are focused on the input and output of the function.
​
It is important to see that our tests fail before we pass them. This way we avoid false positives and make sure that the tests are running correctly.
​
The function only has to do what is required of it by the tests. Writing the minimal amount of code and keeping the RGR loop tight helps us clearly see which parts of our code break stuff. Less code is better! Simple code is easier to debug!
​
As a whole the test-suite will test all of the functionality but implementing one feature at a time makes it significantly easier to solve complex problems.
​
## Writing tests
​
```js
function sumArgs() {
​
}; // <-- this is our unit of code, our CUT
​
console.log(`sumArgs should return 0 when passed no args`);
​
if (sumArgs() !== 0) {
  throw new Error(`sumArgs doesn't return 0`);
} else {
  console.log(`Test passed!`);
}
```
​
The code above satisfies our definition of a test. It invokes the CUT (the function `sumArgs`, in this case) and throws an error if it doesn't give us the desired behaviour (returning `0` when there are no arguments). If there are multiple behaviours then we regard as different test cases - tests that are checking for different behaviours from our CUT. We could have something like the following:
​
```js
function sumArgs() {
  return 0;
}
​
console.log(`sumArgs should return 0 when passed no args`);
if (sumArgs() !== 0) {
  throw new Error(`test failed!`);
} else {
  console.log(`Test passed!`);
}
​
console.log(`sumArgs should return firstArg when passed a single argument`);
if (sumArgs(10) !== 10) {
  throw new Error(`test failed!`);
} else {
  console.log(`Test passed!`);
}
```
​
There are several issues with this approach. Firstly, if an error is thrown at a single point in this code then the whole execution stops and no other test cases can be checked. Secondly, the console output is not formatted nicely. We can refactor further:
​
```js
function sumArgs() {
  return 0;
}
​
try {
  console.log(`sumArgs should return 0 when passed no args`);
  if (sumArgs() !== 0) {
    throw new Error(`test failed!`);
  }
} catch(error) {
  console.log(error);
}
​
try {
console.log(`sumArgs should return firstArg when passed a single argument`);
  if (sumArgs(10) !== 10) {
    throw new Error(`test failed!`);
  }
} catch(error) {
  console.log(error);
}
```
​
Now the `if` statements are wrapped inside `try` blocks and if an error is thrown then the error is caught in the `catch` block. Therefore the rest of the code can execute even if an error has occurred and one of the tests has failed.
​
Finally we can write functions that encapsulate the logic for our approach and keep our tests dry  - saving us the repetition of writing out `try` and `catch` blocks over and over again:
​
​
```js
function expect(actualInput) {
  return {
    toBe: function(expectedOutcome) {
      return actualInput !== expectedOutcome;
    }
  }
};
​
function test(description,func) {
  console.log(description);
  try {
    func();
  } catch (error) {
    console.log(error);
  }
}
​
test(`sumArgs should return 0 when passed no args`,function() {
  expect(sumArgs()).toBe(0);
});
​
test(`sumArgs should return firstArg when passed a single argument`,function() {
  expect(sumArgs(10)).toBe(10);
});
```
​
The functions `expect` and `test` allow us to generalise the logic of our approach. The `test` function "runs the test", it executes our CUT and then catches any errors. The `expect` function performs what is known as an assertion. 
​
## Using jest
​
We can use a popular npm package [jest](https://jestjs.io/en/), a JavaScript testing framework that will allow us to write unit tests easily. The library contains many useful functions like 