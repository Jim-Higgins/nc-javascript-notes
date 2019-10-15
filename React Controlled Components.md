# React Controlled Components
​
## Prior Knowledge
​
- Basic understanding of React components, props and state
​
## Learning Objectives
​
To be able to build a functional form in React by creating a controlled component
​
## React Shopping List
​
Consider the snippets from this Shopping List App below:
​
```js
class App extends React.Component {
  state = {
    ingredients: ['Bread', 'Strawberries', 'Chocolate'],
  };
​
  render() {
    return (
      // rest of App's render
      <ShoppingList ingredients={this.state.ingredients} />
    );
  }
}
​
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
​
To add a new ingredient to our list, we need a `form` - this is going to become complex enough to warrant being made into its own `IngredientAdder` component:
​
```html
<form>
  <label>
    New Task:
    <input type="text" name="newIngredient" />
  </label>
  <button type="submit">Submit new ingredient<button>
</form>
```
​
To cause some action to happen when we submit the form, we can add an `onSubmit` to the form (note that this will allow us to submit the form both by clicking the submit button and by pressing the enter/return key).
​
## handleChange
​
We will want to write a `handleSubmit` function that gets invoked `onSubmit` of the form, but first we must access the user's inputs. It is more convenient to have a JavaScript function that handles the submission of the form and has access to the data that the user entered into the form. The standard way to achieve this is with “controlled components”. [See the docs](https://reactjs.org/docs/forms.html)
​
In HTML, form elements such as `<input>`, `<textarea>`, and `<select>` typically maintain their own state and update it based on user input. In React, if we want to keep hold of some data or information that is going to change the UI, we typically do this in `state`, so form components like `IngredientAdder` need to be _Class Components_.
​
In order to store the user's input into state as they type, we will want a `handleChange` function which gets invoked `onChange` of the input box (note that the React `onChange` event is triggered by any change to the input box straight away, unlike the DOM `onChange` event which only triggers when the input box loses focus). Inside this `handleChange` we can set the `IngredientAdder`'s state to contain the user's input as they type by using the `event.target.value`.
​
```js
class IngredientAdder extends Component {
  state = {
    ingredientInput: '',
  };
​
  render() {
    const { ingredientInput } = this.state;
    return (
      <form>
        <label>
          New Task:
          <input
            type="text"
            name="ingredientInput"
            onClick={this.handleChange}
          />
        </label>
        <button type="submit">Submit new ingredient</button>
      </form>
    );
  }
​
  handleChange = (event) => {
    this.setState({ ingredientInput: event.target.value });
  };
}
```
​
## handleSubmit
​
Now that we have the user's input in the `IngredientAdder`'s state, we can finish writing our `handleSubmit` function. Inside this, we need to make sure to prevent the default behaviour of forms to refresh: `event.preventDefault()`.
​
This `handleSubmit` function needs to be able to update the array of ingredients to have one more ingredient, yet this ingredients array is stored in App's state, not the form's state. In order to add something to App's state, we must invoke `setState` inside the App itself. To do this, we can declare a function `addNewIngredient` in `App.js` and invoke our `setState` inside this function. If we pass this method down to `IngredientAdder`, then `addNewIngredient` can to be called inside our `handleSubmit` function.
​
Note: when doing our `setState` in our `addNewIngredient` function, we need to avoid mutation so rather than invoking `setState` with an object, we must invoke it with an _updater function_ that has access to `currentState`.
​
This gives us the example below:
​
```js
class App extends Component {
  state = {
    ingredients: ['Bread', 'Strawberries', 'Chocolate'],
  };
​
  render() {
    const { ingredients } = this.state;
    return (
      <IngredientAdder addNewIngredient={this.addNewIngredient} />
      <ShoppingList
        ingredients={this.state.ingredients}
      />
      // rest of App's render code
    );
  }
​
  addNewIngredient = (newIngredient) => {
    this.setState((currentState) => {
      return { ingredients: [...currentState.ingredients, newIngredient] };
    });
  };
}
​
// in IngredientAdder.js
​
class IngredientAdder extends Component {
  state = {
    ingredientInput: '',
  };
​
  render() {
    const { ingredientInput } = this.state;
    return (
      <form onSubmit={this.handleSubmit}>
        <label>
          Name:
          <input
            onChange={this.handleChange}
            name="ingredientInput"
            value={ingredientInput}
          />
        </label>
        <button>Submit new ingredient</button>
      </form>
    );
  }
​
  handleChange = (event) => {
    this.setState({ ingredientInput: event.target.value });
  };
​
  handleSubmit = (event) => {
    event.preventDefault();
    const { ingredientInput } = this.state;
    this.props.addNewIngredient(ingredientInput);
    this.setState({ ingredientInput: '' });
  };
}
```