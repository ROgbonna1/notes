# Intro to React
> "React Components are just idempotent functions that describe your UI at any given point in time." - [Pete Hunt](https://www.youtube.com/watch?v=x7cQ3mrcKaY)

## JSX
```javascript
const element = <h1>Hello, world!</h1>;
```
* JSX is a syntax extension to JavaScript. Think of it as a templating language with the full power of JavaScript. JSX produces React "elements".
* Why JSX? React embraces the fact that rendering logic is inherently couple with other UI logic such as event handling logic, state change logic, etc.
* React embraces tight coupling of rendering logic in order to have loose coupling of independent UI units ("components").
* We use curly braces to embed JavaScript expressions in JSX:
      ```javascript
      const name = 'Josh Perez';
      const element = <h1>Hello, {name}</h1>;

      ReactDOM.render(
        element,
        document.getElementById('root')
      );
      ```
* **Note:** You'll often see multiline JSX expressions wrapped in parentheses to prevent automatic semicolon insertion.
      ```javascript
      const element = (
        <h1>
          Hello, {formatName(user)}!
        </h1>
      );
      ```
* JSX expressions become regular JavaScript function calls so we can use them inside of `if` statements, return them from functions, etc.
* JSX prevents injection attacks by default. The React DOM escapes any values embedded in JSX.

## Rendering Elements
* Babel compiles JSX down to `React.createElement()` calls. The following two statements are identical:
      ```javascript
      const element = (
        <h1 className="greeting">
          Hello, world!
        </h1>
      );
      ```
      ```javascript
      const element = React.createElement(
        'h1',
        {className: 'greeting'},
        'Hello, world!'
      );
      ```
  * All of this renders an object called a **"React Element"** that looks something like this:
      ```javascript
      // Note: this structure is simplified
      const element = {
        type: 'h1',
        props: {
          className: 'greeting',
          children: 'Hello, world!'
        }
      };
      ```
  * Unlike DOM elements, React elements are plain objects, and are cheap to create. React does the work of updating the actual DOM to match the React elements.
  * **Note:** Elements are _not_ components. Components are made of elements.

* We render React elements inside of a "root" DOM node. React apps typically have just one root DOM node.
* To render a React element, we pass the element and the root DOM node to `ReactDOM.render()`.
      ```javascript
      const element = <h1>Hello, world</h1>;
      ReactDOM.render(element, document.getElementById('root'));
      ```
* **React elements are immutable**. Once you create an element, you can't change its children or attributes. An element is a snapshot of a UI at a single point in time.

## Components and Props
> Components let you split the UI into independent, reusable pieces, and think about each piece in isolation.
* Components are like JavaScript functions: they accept arbitrary inputs, _props_, and return React elements describing what should be rendered in the UI.

### Function Components
```javascript
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```
* The above function is valid React component. It takes an object argument, `props` and returns a React element.

### Class Components
* We can use ES6 Classes to create React components.
```javascript
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

* Elements can represent user-defined components:
```javascript
const element = <Welcome name="Sara" />;
```

* Components can refer to other components in their output, allowing them to be composable.
* **All React Components but act like pure functions with respect to their props.**
