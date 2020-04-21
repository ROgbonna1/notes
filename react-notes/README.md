# Intro to React
> "React Components are just idempotent functions that describe your UI at any given point in time." - [Pete Hunt](https://www.youtube.com/watch?v=x7cQ3mrcKaY)

## JSX
```javascript
const element = <h1>Hello, world!</h1>;
```
* JSX is a syntax extension to JavaScript. Think of it as a templating language with the full power of JavaScript. JSX produces React "elements".
* Why JSX? React embraces the fact that rendering logic is inherently coupled with other UI logic such as event handling logic, state change logic, etc.
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

### Rendering Components


* Components can refer to other components in their output, allowing them to be composable.

* **Props are Read-Only.** A component may never modify its own props.
* **All React Components must act like pure functions with respect to their props.**
**Note:** I'm slightly confused about this quote. Is this a suggestion or a rule? This is an example of component that does indeed mutate its props:
```javascript
const MutableComponent = (props) => (
  <h1>
    Component Balance: {props.account.balance -= 10}
  </h1>
); 

const myAccount = { balance: 100, transactions: 0}
ReactDOM.render(
  <div>
    <MutableComponent account={myAccount} />
    <MutableComponent account={myAccount} />
  </div>,
  domContainer
);

console.log(myAccount);
```

#### Learning Snippet
The following snippet renders "Hello, Rhea" to the DOM.
```javascript
const Greeting = (props) => <h1>Hello, {props.name}</h1>;
ReactDOM.render(Greeting({ name: 'Rhea' }), domContainer);
```

* Elements can represent user-defined components. An element that has the tag of a user-defined component is like an invocation of the function that returns a React element. When using user defined components in JSX, JSX attributes and children of this component are passed as a single _props_ object. Contrast the snippet below with the one above.
```javascript
const Greeting = (props) => <h1>Hello, {props.name}</h1>;
ReactDOM.render(<Greeting name='Rhea' />, domContainer);
```

### Composing Components
* Components are composable. Example:
```javascript
const domContainer = document.getElementById('react_container');

const Greeting = (props) => <h1>Hello, {props.name}</h1>;

const FraternityGreetings = ({ prophytes }) => (
  <div>
    <Greeting name={prophytes.dean} />
    <Greeting name={prophytes.adp} />
    <Greeting name={prophytes.oldHead} />
  </div>
);

const myProphytes = {
  dean: 'Zach Stanfill',
  adp: 'Karl Analo',
  oldHead: 'Cyril Broderick',
};

ReactDOM.render(<FraternityGreetings prophytes={myProphytes} />, domContainer);
```

### Extracting Components
Because components are composable, we can refactor large components into a composition of smaller components:
Before:
```javascript
function Comment(props) {
  return (
    <div className="Comment">
      <div className="UserInfo">
        <img className="Avatar"
          src={props.author.avatarUrl}
          alt={props.author.name}
        />
        <div className="UserInfo-name">
          {props.author.name}
        </div>
      </div>
      <div className="Comment-text">
        {props.text}
      </div>
      <div className="Comment-date">
        {formatDate(props.date)}
      </div>
    </div>
  );
}
```

After:
```javascript
const Avatar = ({ user }) => (
  <img className="Avatar"
    src={user.avatarUrl}
    alt={user.name}
  />
);

const UserInfo = ({ user }) => (
  <Avatar user={user} />
  <div className="UserInfo-name">
    {user.name}
  </div>
);

const Comment = ({author, date, text}) => (
  <UserInfo user={author} />
  <div className="Comment-text">
    {text}
  </div>
  <div className="Comment-date">
    {date}
  </div>
);
```

## Lifecycle
* The first time a component is rendered, we say that it has "_**mounted**_".
* When a component is removed,is is "_**unmounted**_".

## Hanldling Events
Handling events with React elements is very similar to handling events on DOM elements. There are some syntax differences:

* With JSX you pass a function as the event handler, rather than a string.
```javascript
<button onClick={activateLasers}>
  Activate Lasers
</button>
```

* Another difference is that you cannot return false to prevent default behavior in React. You must call `preventDefault` explicitly.
```javascript
function ActionLink() {
  function handleClick(e) {
    e.preventDefault();
    console.log('The link was clicked.');
  }

  return (
    <a href="#" onClick={handleClick}>
      Click me
    </a>
  );
}
```
When using React, you generally don’t need to call `addEventListener` to add listeners to a DOM element after it is created. Instead, just provide a listener when the element is initially rendered.

## Conditional Rendering
Conditional rendering in React works the same way conditions work in JavaScript. Use JavaScript operators like `if` or the conditional operator to create elements representing the current state, and let React update the UI to match them.

```javascript
const UserGreeting = () => <h1>Welcome Back!</h1>

const GuestGreeting = () => <h1>Please Sign Up</h1>

const HomePage = () => {
  const [isLoggedIn, setLogIn] = useState(false);

  if (isLoggedIn) {
    return <UserGreeting />;
  };

  return <GuestGreeting />;
};
```
