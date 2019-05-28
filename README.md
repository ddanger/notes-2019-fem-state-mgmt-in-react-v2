# notes-fem-state-mgmt-in-react-v2
Notes on the frontend masters workshop "State Management in React, v2" by Steve Kinney

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
