notes-fem-state-mgmt-in-react-v2

Notes on the frontend masters workshop "State Management in React, v2" by Steve Kinney

# Day 1: State in React
- While the React API seems to change a lot, this resource is really good and more timeless for [Thinking in React](https://reactjs.org/docs/thinking-in-react.html)
- State vs. Props: your state may become a childs props. Or another way to say it, your props may be somebody else's state.
- Interesting `setState` variations
  - pass update function, should be done if new state is dependent on prior state
  - pass second arg, a callback to run after setting the state, e.g. update localStorage
- Don't put something in `state` that can be computed from props. Just compute it from props
- ProTip: You can use the `get` keyword in front of a class method and then in your render just reference a computed value as if it's a property instead of a function call.  (e.g.
```javascript
class Sample extends React.component {

  state = {
    stateItem: 'stuff'
  };
  
  get computedValue() {
    return this.props.aProp + this.state.stateItem;
  }
  
  render() {
    {/*
      no need to call a function here 
      reminds me of Vue `computed`
      but in React I'm not sure it's a good idea to hide the fact that it's a function
    */}
    <div>{this.computedValue}</div>
  }
}
```
- "The prop-drilling struggle is real!"
- "You can document what your intent was by using a well-named function"
- Lowest common ancestor often doesn't work in practice, because state tends to end up on the root
## Mitigating the prop-drilling problem
- Higher order components
    - allow you to wrap a component with the state/callbacks
- Render props
    - another way to wrap component with state/callbacks
- Context API
- Hooks. What are hooks? Hooks are a new api for managing react state that's completely different from the old APIs. yay!
  - useReducer cool for form validation or in any case where various pieces of state interact with each-other
  - Custom hook useLocalStorage is cool
  
# Day 2: redux, redux-thunk, redux-observable, mobx
## Redux
There isn't much to it.

### Vocab
- reducer - a function which takes state and action and returns new state  
  - don't mutate the state. Must return new state.
- action - javascript object with a `type` and a `payload` and maybe a `meta`
- action creator - a function to return an action (and probably take an argument or two)
- store - the thing that holds your state tree and exposes functions to get it and dispatch actions to change it
- state - the snapshot of the current data in the store

### Functions
- createStore - accepts a reducer function and optionally middleware
  - getState - returns the current state
  - dispatch - send (dispatch) an action into the reducer. action has a type and a payload and optionally meta
  - subscribe - you pass a function in, and it will be called after every dispatch
  - replaceReducer - replaces the reducer your store is using. Helpful if you're code-splitting or loading parts of your app on the fly
- combineReducers - often for maintainability you create multiple small reducers for different branches of the state tree. But you need to combine them prior to creating your store. But despite this, the actions you dispatch will flow through all reducers.
- compose - creates a new function that is the functional composition of its arguments)
- bindActionCreator, bindActionCreators - optionally used to reduce boilerplate. Just creates a special-purpose function you can use to dispatch a particular action
- applyMiddleware - used to inject code into the lifecycle of redux to do stuff based on what's happening

## Redux
- mapStateToProps - function that takes the state and returns something to pass to the component as props
  - choose the state data you need. If you pass too much, you'll get unnecessary re-renders
- mapDispatchToProps - there are various formats. Ultimately we use a real simple one that just returns the action creators
- connect

## Setting Up Data
- Prefer objects over arrays `{ 1: { data: 'stuff' }, 2: { data: 'more stuff } }` instead of `[ { data: 'stuff' }, { data: 'more stuff' } ]`

## Selectors and Reselect
- reselect - memoization 

## Redux Thunk
- actiion creator returns a function that takes a `dispatch` param. It does async stuff and then calls `dispatch`.

## redux-observable
- epic - function that takes a stream of actions and returns a stream of actions.
  - Allows you to transform the action stream.
  - It's cool becuase it enables cancellation of async stuff.

## MobX
It's like Vue.js but more complex.
- computed properties
- decorators
