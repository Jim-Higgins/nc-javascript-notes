# React Intro

## Prior Knowledge

- Understanding and awareness of basic HTML elements
- DOM tree structure
- DOM manipulation
- Javascript (especially classes)

## Learning Objectives

To be able to build a basic single page application using React through and awareness and understanding of:

- benefits of using React
- what JSX is
- how to render JSX elements with React
- what a component is and how to render them
- `props` and `state` in React components
- conditional rendering

## React JS

> "A JavaScript library for building user interfaces" - [React Homepage](https://reactjs.org/)

### Why React ?

Making changes through DOM manipulation can be slow as well as difficult.

- The DOM is relatively slow to update due to it's tree structure, which requires recursion to traverse.

- React gives developers the ability to work with a _virtual DOM_, removing the need to use the browser's inbuilt DOM methods.

- A benefit of this is that it allows us to be _declarative_ when building our User Interface (UI) using JavaScript. This means that rather than having to describe each step in the transition to an updated UI using DOM methods, we can simply write the final version of the page that should be displayed with React.

- The _virtual DOM_ used by React is much faster and more efficient to update, as instead of manually traversing through the parent and child nodes of the DOM, the _virtual DOM_ will check itself against the previous version and only update what is necessary, based on the updated UI that has been declared.

- As a result, React makes it easy to "hydrate" a page with data from API requests on the client side.

## Basic React Set-up

The file structure of a typical React application will something like this:

```raw
├── README.md
├── package-lock.json
├── package.json
├── public
│   ├── favicon.ico
│   ├── index.html
│   └── manifest.json
└── src
    ├── App.css
    ├── App.js
    ├── App.test.js
    ├── index.css
    └── index.js
```

In the majority of React apps that you see and create, there will be a `div` with an `id` of `root` within an `index.html` file. Typically, `index.html` will be found inside a public folder as can be seen in the above folder structure.

```html
-- the rest of the html file...
<div id="root"></div>
-- html file continued...
```

This is referred to as the “root” DOM node because everything inside it will be managed by React, and this is where our React apps will be rendered (displayed).

Within an `index.js` file, we can render a React element inside the root DOM node using `ReactDom.render()`:

```js
import React from 'react';
import ReactDOM from 'react-dom';

const element = <h1>Hello, world</h1>;
ReactDOM.render(element, document.getElementById('root'));
```

The `element` that has been created above looks just like a HTML element, but the above example is a blend of JavaScript and HTML syntax, this is in fact a syntax extension of JavaScript called JSX.

JSX effectively allows us to write HTML syntax inside our JavaScript files. See [React Intro to JSX](https://reactjs.org/docs/introducing-jsx.html) for more info.

JSX can only be used in this file because `react` has been imported.

### Babel

Notice the way that the `react` and `react-dom` packages are required (or in this case _imported_) into the file. This syntax is made possible by [Babel](https://babeljs.io/repl), a JavaScript compiler, which compiles our code down to browser compatible JavaScript. The `import` syntax is modern ES6 JavaScript which is converted to Common JS `require` under the hood.

Some more details on the difference between `import` & `require` courtesy of [Stack Overflow](https://stackoverflow.com/questions/31354559/using-node-js-require-vs-es6-import-export)

Babel can also be used to show why we can create a variable that looks like an HTML element when using React:

```js
const element = <h1>Hello world</h1>;
```

Compiles down to:

```js
var element = React.createElement('h1', null, 'Hello world');
```

The fact that `<h1>Hello world</h1>` compiles to `React.createElement('h1', null, 'Hello, world')` is why `React` must be imported in order for the JSX syntax to work.

## React Elements

JavaScript can be embedded within JSX using curly braces. E.g.

```js
const sum = <p>1 plus 2 = {1 + 2}</p>; // `{1 + 2}` will be evaluated to 3
ReactDOM.render(sum, document.getElementById('root'));
```

See [Embedding Expressions in JSX](https://reactjs.org/docs/introducing-jsx.html#embedding-expressions-in-jsx) for more info.

In order to make the above `sum` example more _flexible_ and _reusable_ we can create a _function_.

```js
const sum = (num1, num2) => <p>1 plus 2 = {num1 + num2}</p>;
ReactDOM.render(sum(1, 2), document.getElementById('root'));
```

_**However** components should **NEVER** be invoked like functions_

## React Components

_Components should **ALWAYS** start with an upper-case letter_

This allows us to call the component using tags (i.e. `<ComponentName />`) without this notation being confused for HTML elements / DOM tags.

This is because React treats components starting with lowercase letters as DOM tags. For example, `<input />` represents an HTML input tag, but `<Sum />` represents a call to the component `Sum`. See [JSX in depth](https://reactjs.org/docs/jsx-in-depth.html#user-defined-components-must-be-capitalized) for more info.

## Props

Props are the means by which we can pass data to our React components.

Only functions that accept a single "props" object argument (short for properties) and return a _single_ React element are valid React Components. These are called “function components” as they are literally JavaScript functions.

```js
const Sum = (props) => <p>1 plus 2 = {props.num1 + props.num2}</p>;
ReactDOM.render(<Sum num1={1} num2={2} />, document.getElementById('root'));
```

Props are read-only and should not be mutated or modified directly by the component.

See the [docs](https://reactjs.org/docs/components-and-props.html#props-are-read-only) for more.

## Child Elements

Although a component can only return a _single_ React element, the element _CAN_ have child elements. In the example below the `ul` element has 4 `li` children nested within it.

```js
const RockSchedule = (props) => {
  return (
    <ul>
      <li>One</li>
      <li>Two</li>
      <li>Three o'clock</li>
      <li>{props.newListItem}</li>
    </ul>
  );
};
ReactDOM.render(
  <RockSchedule newListItem="Four o'clock rock" />,
  document.getElementById('root')
);
```

More on [Specifying Children with JSX](https://reactjs.org/docs/introducing-jsx.html#specifying-children-with-jsx)

## Displaying Lists and Keys

We can use `.map` to return an array of elements. See the React docs for more info on [Lists and Keys](https://reactjs.org/docs/lists-and-keys.html) and why we must use a unique `key` prop.

```js
const RockSchedule = (props) => {
  return (
    <ul>
      {props.listItems.map((item, i) => {
        return <li key={i}>{item}</li>;
      })}
    </ul>
  );
};
ReactDOM.render(
  <RockSchedule listItems={['One', 'Two', 'Three', "Four o'clock rock"]} />,
  document.getElementById('root')
);
```

On line 167, we have to pass through an array of `listItems` as a prop which will then be mapped over by each string in order to produce an array of corresponding `li` elements.

## Splitting up our code into separate components

Components allow us to split our apps into independent and reusable units. A major benefit of separating our code into separate components is that we can then begin to reason about each component in isolation.

E.g. splitting out the header to a separate component:

```js
const App = (props) => {
  return (
    <div>
      <Header />
      <RockSchedule listItems={['One', 'Two', 'Three', "Four o'clock rock"]} />
    </div>
  );
};

const Header = (props) => (
  <header>
    <h1>Rocking around the clock</h1>
  </header>
);

const RockSchedule = (props) => {
  return (
    <ul>
      {props.listItems.map((item, i) => {
        return <li key={i}>{item}</li>;
      })}
    </ul>
  );
};

ReactDOM.render(<App />, document.getElementById('root'));
```

## Class Components

You can also use an ES6 class to define a component. Read more in the [React Docs](https://reactjs.org/docs/components-and-props.html#function-and-class-components).

By default we should make our components **functional** unless we find the need for one of the additional features provided by a class component.

class components have some additional features:

### `render()` method

All class components must have a [`render()`](https://reactjs.org/docs/react-component.html#render) method. The `render` method should typically return React elements.

The syntax for creating a Class Component is as follows:

```js
class App extends React.Component {
  render() {
    return (
      // React element
    );
  }
}
```

### `this.props`

Classes in JavaScript are essentially constructors under the hood and when building a new instance inside a class we still use the `this` keyword to point to the new instance that is being created. Therefore, when trying to access `props` on a class based component we actually need to reference `this.props`.

## State

React class components also have a property called `state`. This state is private to a specific component and is controlled and updated by the same specific component. We expect the state of a component to change over time and this change will normally trigger an update in what is displayed on the page.

It is where a component can keep data that may change which will affect what is displayed on the page.

For example, in an app designed to hold a shopping list, the array of ingredients that could be passed down to a `ShoppingList` component:

```js
class App extends React.Component {
  state = {
    ingredients: ['Bread', 'Strawberries', 'Chocolate'],
  };
  render() {
    return (
      <div>
        <h1>Shopping List</h1>
        <ShoppingList ingredients={this.state.ingredients} />
      </div>
    );
  }
}

const ShoppingList = (props) => {
  return (
    <ul>
      {props.ingredients.map((ingredient) => {
        return (
          <li key={ingredient}>
            <p>{ingredient}</p>
          </li>
        );
      })}
    </ul>
  );
};
```

We would choose to have `ingredients` on `state` in this example as we may want to add or remove ingredients when someone is interacting with the application. Any update of the UI should reflect a corresponding update of a component's state.

Read more about React state in the [docs](https://reactjs.org/docs/react-component.html#state)

### Updating State with `setState()`

If we want to change the way that something appears or behaves we can store information in state, and update this information when something specific happens (e.g. a 'click' or other event).

To do this we use `.setState`, which is a method available within `class` components.

The following would set the state of `name` to a string of 'Paul'.

```js
this.setState({ username: 'Paul' });
```

When `setState()` is called, React performs a shallow merge of the object that is provided into the current state. This means only the specified key (`username` in this example) is updated, leaving the unspecified keys unchanged.

We cannot, and _must not_ update state directly. We must always use `.setState()` rather than trying to reassign state, or a property of state!

This is because simply reassigning state using `this.state = // insert something here` does not cause the `render()` method to be called. And as such, no changes that were intended to happen following an update to `state` would take effect.

More on `setState` in [the docs](https://reactjs.org/docs/react-component.html#setstate)

### Class Component Methods

To make use of `setState` we can add methods to our Class Components.

For example, in our Shopping List App, we may want to show which ingredient is the most important to buy. We might want to keep a track of which is the 'priorityIngredient' in state, and be able to change which is the priority ingredient. We would need to write a method that updates this priorityIngredient using `setState`.

This method then needs to be passed down to the `ShoppingList` component where it could be invoked when a 'Make this the priority ingredient' button is clicked.

```js
class App extends React.Component {
  state = {
    ingredients: ['Bread', 'Strawberries', 'Chocolate'],
    priorityIngredient: '',
  };

  render() {
    return (
      <div>
        <h1>Shopping List</h1>
        <p>Priority ingredient is: {this.state.priorityIngredient}</p>
        <ShoppingList
          ingredients={this.state.ingredients}
          updatePriorityIngredient={this.updatePriorityIngredient}
        />
      </div>
    );
  }

  updatePriorityIngredient = (ingredient) => {
    this.setState({ priorityIngredient: ingredient });
  };
}

const ShoppingList = (props) => {
  return (
    <ul>
      {props.ingredients.map((ingredient) => {
        return (
          <li key={ingredient}>
            <p>{ingredient}</p>
            <button onClick={() => props.updatePriorityIngredient(ingredient)}>
              Make this the priority ingredient
            </button>
          </li>
        );
      })}
    </ul>
  );
};
```

Note that in order to use `this.setState` the `updatePriorityIngredient` method uses arrow function syntax. This syntax ensures that the `this` value of the method is bound to the instance or component that has been created, therefore wherever this function is called its `this` value will point to the original instance that was created. In doing so, we can pass this bound function around to another child component and be guaranteed that its `this` value will still refer to the parent component where it was created.

#### An aside about onClick

`onClick` takes a _function_.

If we pass onClick `props.updatePriorityIngredient`, this function will be called with a synthetic `event` when triggered that is created by React (but for all intended purposes, the same as the 'real' event).

We aren't really interested in this event (we could inspect it to try to find out the `event.target`, but the data we need is already available to us in React!).

If we were to pass `props.updatePriorityIngredient(ingredient)` to the `onClick`, the `onClick` is no longer being passed a _function_, but a _function invocation_. So as soon as we mount this component, this function will be called.

The solution is to wrap our `updatePriorityIngredient` function in an anonymous function that invokes it with the correct argument(s).

#### setState updater function

In the above example, `setState` is invoked with an object: `this.setState({ priorityIngredient: ingredient });`. If ever we need to update state based on a value that is currently in state, then we must invoke `setState` with an _updater function_ that returns the required state change.

For example, in our Shopping List App, if clicking an ingredient sets it as the `priorityIngredient`, then we might want to be able to click it again to remove it from being the `priorityIngredient` (reset the `priorityIngredient` back to an empty string). Therefore, we need to know what value was at `priorityIngredient` the exact time that setState was triggered.

When we invoke `setState` with an updater function, this function will receive as its first argument the current state at the time that setState is executed. This means that we can check the `priorityIngredient` key on this `currentState` object to see whether it matches the new ingredient or not.

```js
updatePriorityIngredient = (ingredient) => {
  this.setState((currentState) => {
    return {
      priorityIngredient:
        currentState.priorityIngredient === ingredient ? '' : ingredient,
    };
  });
};
```

### Conditional Rendering

You might want to display a component - or elements within a component - only sometimes, dependent on the state of your application. The React documentation calls this [conditional rendering](https://reactjs.org/docs/conditional-rendering.html).

`jsx` relies on return values, so imperative code like `if else` and `for` loops cannot be used.

```js
// this is not valid javascript and will give you a TypeError
const happy = if(true){
  return true
} else {
  return false
}
```

You can achieve conditional rendering inline with ternary and logic operators are expressions. Consider this example where we might want to render the `<p>Priority ingredient is: {this.state.priorityIngredient}</p>` if there actually _is_ a priority ingredient (i.e. if `priorityIngredient` is not an empty string).

```jsx
{
  this.state.priorityIngredient ? (
    <p>Priority ingredient is: {this.state.priorityIngredient}</p>
  ) : null;
}
```

The logical `and` operator can achieve the `if` without an `else`:

```jsx
// within Quote render
{
  this.state.priorityIngredient && (
    <p>Priority ingredient is: {this.state.priorityIngredient}</p>
  );
}
```

This makes our example resemble the following:

```js
//... the rest of App.js
  render() {
    return (
      <div>
        <h1>Shopping List</h1>
        { this.state.priorityIngredient && <p>Priority ingredient is: {this.state.priorityIngredient}</p>}
        <ShoppingList
          ingredients={this.state.ingredients}
          updatePriorityIngredient={this.updatePriorityIngredient}
        />
      </div>
    );
  }

```