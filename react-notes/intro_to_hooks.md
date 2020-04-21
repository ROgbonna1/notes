# Introduction to Hooks
> _Hooks_ are a new addition in React 16.8. They let you use state and other React features without writing a class.

### Why _Hooks_?
* It's hard to reuse stateful logic _between_ components?
  * "wrapper hell" of components surrounded by layers of providers, consumers, higher-order components, render props, and other abstractions.
  * "react needs a better primitive for sharing stateful logic"
* Complex components become hard to understand.
  * "Hooks let you split one component into smaller functions based on what pieces are related"
* "Classes confuse both people and machines"
  * classes are verbose
  * `this` is confusing
  * _The distinction between function and class components in React and when to use each one leads to disagreements even between experienced React developers._
* _"classes don’t minify very well, and they make hot reloading flaky and unreliable."_

>To solve these problems, Hooks let you use more of React’s features without classes. Conceptually, React components have always been closer to functions. Hooks embrace functions, but without sacrificing the practical spirit of Reac

### Two Rules of Hooks
* Only call Hooks at the top level. Don’t call Hooks inside loops, conditions, or nested functions.
* Only call Hooks from React function components. Don’t call Hooks from regular JavaScript functions. (There is just one other valid place to call Hooks — your own custom Hooks. We’ll learn about them in a moment.)

## `useState`
#### Class Component Counter (pre-Hooks)
```javascript
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

#### Function Component w/ `useState` Hook
```
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

* Invoking `useState` returns a pair of values (as an array that we can deconstruct): (1) the current state and (2) a function that updates the value:
```javascript
import React, { useState } from 'react';

function Example() {
  // Declare a new state variable, which we'll call "count"
  const [count, setCount] = useState(0);
``` 

* **Reading State:** When we want to read from state, we directly reference the first element that `useState` returns.
```javascript
  <p>You clicked {count} times</p>
```

* **Updating State:** The second argument that `useState` returns (`setCount` in our case) reassigns the state a new value based on whatever value it (`setCount`) is invoked with.
```javascript
<button onClick={() => setCount(count + 1)}>
    Click me
</button>
```
  * We can also pass `setState` a callback function. The return value when this callback function is invoked will be the new state value.
  ```javascript
  <button onClick={() => setCount(count + 1)}>
    Click me
  </button>
  ```
