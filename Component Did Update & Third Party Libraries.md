# Component Did Update & Third Party Libraries
​
## Prior Knowledge
​
- be comfortable getting external data into React state using fetch
- know how to structure & pass data around a React app
- be comfortable with the 'isLoading' pattern
​
## Learning Objectives
​
- be able to use axios for data requests
- understand use cases for additional React lifecycle methods
- know when each React lifecycle method will be called
- get comfortable configuring third party React libraries
​
## Axios
​
`Axios` (https://github.com/axios/axios) is a Promise-based third party package used to make HTTP requests for external data, similar to the `fetch` api.
​
```js
axios.get(url).then(({ data: { items } }) => {});
```
​
The table below outlines some of the key similarities and differences between `fetch` and `axios`.
​
| Fetch                                                        | Axios                                         |
| ------------------------------------------------------------ | --------------------------------------------- |
| Promise based                                                | Promise based                                 |
| Native - no installation needed                              | Need to install                               |
| Buffer initially returned - need json method                 | Response initially returned                   |
| Only response body returned after json method                | More of response available                    |
| Body directly available                                      | Body only available on data property          |
| Request method supplied in options object `{method: 'POST'}` | request methods available e.g. `axios.post()` |
| request body supplied in options object                      | request body supplied in options object       |
​
## Component Did Update
​
[componentDidUpdate](https://reactjs.org/docs/react-component.html#componentdidupdate) method is not called for the initial render. This method is called (by React) after every subsequent render. It will be invoked with two important arguments:
​
- prevProps (the props as they were _before_ the component re-rendered)
- prevState (the state as it was _before_ the component re-rendered)
​
```js
componentDidUpdate(prevProps, prevState) {
​
};
```
​
`componentDidUpdate` is useful for carrying out an action based when some other data in the application changes. A common use case could be fetching new data lower down the component tree if some prop above changes.
​
Because `setState` triggers a re-render (which in turn causes to be `componentDidUpdate` invoked), it is vital that any call to `setState` in a `componentDidUpdate` is wrapped in an appropriate condition to avoid an infinite loop (**Remember:** non-primitive data types will be compared by reference!).
​
```js
componentDidUpdate(prevProps) {
  // Typical usage (don't forget to compare props):
  if (this.props.userID !== prevProps.userID) {
    this.fetchData(this.props.userID);
  }
}
```