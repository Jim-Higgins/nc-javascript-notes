# Front-end Testing & Cypress
​
## Prior Knowledge
​
- Understand the nested structure of the DOM.
- Be able to use DOM functions to select and create & manipulate html elements.
- Understand what events are and how to use event listeners.
​
## Learning Objectives
​
- Understand how front-end testing differs from other types of testing.
- Understand how to use cypress methods to construct tests.
- Be able to utilise best practices and use data-sets when testing.
- Be able to use TDD when building front-end projects.
- Improve understanding of DOM manipulation and event handlers.
​
## Types of testing:
​
|    Type     |                                     Description                                      |
| :---------: | :----------------------------------------------------------------------------------: |
|    Unit     |             No api requests. A single function with an input and output              |
| Integration |                               multiple units together                                |
| Regression  | testing over time confirms that changes to the code base do not break other features |
| End to end  |             Flow-based testing e.g. login, add item to basket, check out             |
​
> Some other types include : performance, penetration, a11y, i18n, smoke, usability, stress, fuzz...
​
---
​
So far on the course we have used **unit** testing. We have used mocha to run those tests and chai as an assertion library, for building our `expect` statements. You'll have also used Jest, which performs the role of both test runner and assertion library.
​
# Cypress
​
Cypress is an end to end / integration testing tool that runs in the browser. Under the hood it uses `Mocha` and `Chai`, so a lot of the syntax is familiar.
​
Integration testing has a different approach to unit testing we are used to. Integration testing expects a certain 'narrative' to be followed - you are describing a use case in your tests which might encompass several steps.
​
## 1. Setup
​
Taken from the [cypress docs](https://www.cypress.io/) :
​
---
​
1. `cd` into frontend project
2. `npm init`
3. `npm i -D cypress` - cypress is a developer dependency
4. add script to package.json `"cy:open": "cypress open"`
5. `npm run cy:open`
​
> This opens up the cypress test runner and creates a cypress folder in your project directory.
​
- All tests will be run from the `cypress/integration` folder.
​
- You can see Cypress running tests on their own site if you explore `./cypress/integration/examples`.
​
- The cypress runner will only re-run tests automatically if something has changed in the `integration` folder.
​
## 2. Plan
​
It's always good practice to start with a plan & a diagram of what you're going to create. It helps with a TDD to approach building the site. Ideas and approaches change more often at the front-end so when designing a test suite some aspects of TDD may not be so applicable.
​
## 3. Writing Tests
​
1. Create a file inside the integration folder `./cypress/integration/<app-name>.spec.js` - this is where the cypress will run your tests from
​
Like Mocha, in `cypress` you can use `describe` and `it` blocks. We can also use `expect` without `requiring` it in - cypress makes it globally available.
​
2. Write a test - a smoke test that will definitely pass / fail to make sure things are working. e.g.
​
```js
describe('Our first test', () => {
  it('shows us a failing test', () => {
    expect(true).to.equal(false);
  });
});
```
​
### **Testing Principles**
​
---
​
> TDD principles still apply, you can plan your site but still write your tests before you write any HTML or JS!
​
_Our Front-End tests, like end-to-end tests are going to have to follow a narrative of a chain of events or user interaction. Often the narrative will follow this kind of flow:_
​
1. Visit website
2. Interact with items on the page
3. Make test assertion
​
## 4. Cypress Methods
​
### **Visiting websites**
​
The first task of any test will be to "visit" the site.
​
- The `cy.visit` method allows you to pass a string url to a specific site.
- If `cy.visit` is called with an empty string, cypress will look for an `index.html` page in the root directory.
​
- If `cy.visit` can't find the `index.html` or `url` of your chosen site, _the test will fail_
​
### **Interacting with items on the page**
​
Like DOM if we want to make any assertions about our page or interact with any items of our page - we need to _select_ them first.
​
#### GET
​
- `cy.get()` is our primary method for isolating particular elements so we can test their state. It takes a [css selector](https://www.w3schools.com/cssref/css_selectors.asp) argument (i.e. the same string you can use to apply css). e.g.
​
```js
cy.get('#list');
```
​
- `cy.get()` will get the _first_ thing it finds of that selector in the DOM.
- This makes it necessary to label your html elements properly - you don't want to accidentally _test_ or _interact_ with the wrong item.
​
#### DATA SETS
​
_Using IDs (`#`) and classes (`.`) is not best practice as specified on the [cypress docs](https://docs.cypress.io/guides/references/best-practices.html)._
​
We do not to want to adapt our tests as we change the contents of our page. We also don't want to couple our testing too closely with the styling or functionality of our site.
​
A solution that avoids this is to use the `data attribute` of an html tag. An element can be given multiple data attributes, which are encompassed in a [dataset property](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset).
​
- Datasets advertise clearly what the attribute is for e.g. `data-<usefulName>='someValue'`
- For testing we can use `data-cy`
- This prevents conflict with other attributes which are used for other purposes i.e. using classes for CSS.
​
If we want a test to interact with an input box we could add a data attribute like so:
`<input data-cy="task-adder" />`
​
To `get` this item for our test you use [css attribute selectors](https://www.w3schools.com/cssref/sel_attribute_value.asp):
​
`cy.get('[data-cy=task-adder]')`
​
### **Assertions**
​
There are lots of [different assertions available](https://docs.cypress.io/guides/references/assertions.html#Chai)
​
- Chai `equal` / `eql` assertions are still available in cypress as well as all your other chainables like `have.all.keys` or `contains`
- You can also use the cypress `should` which takes 2 arguments:
​
  - A chain of chai assertions (seperated by dots) as its first argument
  - The value of the `equal to` as its second argument
  - e.g.
    `should(have.length, 4)`
​
**When to assert**
​
Much like back-end testing, be wary of **when** you want to make your assertion. In back-end, you have to mock a request and wait for data to come back before you make your final assertions. In front-end make sure your full narrative has completed - you have selected and interacted with all the elements you need to before you make your assertions.
​
> **Testing an input box adds to a list based on list length**
​
_What won't work_
​
Here the input box being typed in is not the list which is being checked for the test - _the input box's length will only ever be 1_.
​
```js
// A not so useful command chain.
cy.get('[data-cy=input-box'])
  .type('new item {enter}')
  .should('have.length', 4)
```
​
_What will work_
​
To type in the box and check the length of the list you have to query, interact with, and assert on each item individually
​
```js
cy.get('[data-cy=input-box'])
.type('new item {enter}');
​
cy.get('[data-cy=list'])
.children()
.should('have.length', 4)
```
​
### **Chaining**
​
In `Cypress`, don't worry about isolating each action and saving to a variable - instead you can see chaining of assertions or actions.
​
```js
// Example test chaining 2 selection commands and 2 assertions
​
cy.get('[data-cy=item'])
  .children()
  .should('have.length', 1)
  .contains('test text')
```
​
### **Actions & Interactions**
​
- Tests act as the client interacting with the webpage. Sometimes the client will interact with the page by clicking or typing. Part of testing is to make sure the appropriate things happen as a result of those interactions.
​
Cypress allows a variety of action `commands` for tests to interact with a web page.
​
```js
cy.get('[data-cy=task-button]').click();
```
​
```js
cy.get('[data-cy=task-input]').type(`adding a new task {enter}`);
```
​
## 5. DRY testing and Custom Commands
​
If you're repeating steps though, take advantage of mocha's `beforeEach`. We can move `cy.visit("")` there as that's something that needs to happen in each test.
​
Repeating of the same interacting like `click` or `type` - that we don't want to do in test will prevent our code being DRY.
​
Cypress utilities allows the creation of [custom cypress commands](https://docs.cypress.io/api/cypress-api/custom-commands.html#Syntax). Tailored commands allow you to quickly repeat a narrative that multiple tests will need.
​
You can declare a global custom command inside `./cypress/support/commands.js`. This file gives instructions and examples on creating your own command.
​
e.g. entering an item into a list.
​
```js
// inside ./cypress/support/commands.js
Cypress.Commands.add('addItem', item => {
  cy.get('[data-cy=task-adder]').type(`${item}{enter}`);
});
```
​
To use your own custom commands, you can call them in your tests as they are globally available. Refactoring our test code:
​
```js
// inside test
cy.addItem('stuff');
```
​
---
​
# 6. Resources
​
- Check out the [ES6 Mocha Snippets extension](https://marketplace.visualstudio.com/items?itemName=spoonscen.es6-mocha-snippets) for writing quick tests.
- [Cypress docs](https://www.cypress.io/)
- [HTML datasets](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/dataset)
- [Cypress best practices](https://docs.cypress.io/guides/references/best-practices.html)
- [Cypress custom commands](https://docs.cypress.io/api/cypress-api/custom-commands.html#Syntax)
- [CSS selectors](https://www.w3schools.com/cssref/css_selectors.asp)