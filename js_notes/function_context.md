# JavaScript Function Context
Functions are _first-class citizens_ in JavaScript, meaning we can add them to objects, execute them in the context of those objects, remove them from their objects, pass them to other functions, and run them in entirely different contexts.

First-class functions initially have no context; they receive one when the program executes.

## The Global Object
* JavaScript creates a **global object** when a program first runs. This global objects serves as the implied execution context. In the browser, the global object is the `window` object.
* When we talk about _global values_ (`NaN`, `Infinity`) and _global functions_ (`parseInt`, `isNaN`), they are properties and methods of the `window` objects.
* As with other JavaScript objects, you can add properties to the global object at any time:
```javascript
window.testProp = 'this is a test';
window.testProp; // 'this is a  test';
```

## The Global Object as Implicit Context
```javascript
// Beginning of program
testFoo = 'This is a global property';
testFoo; // 'This is a global property';
```
In the above example, even though `testFoo` wasn't declared with `var`, `let`, or `const`, it does not produce a `ReferenceError` when called. That is because it is bound to the global object as a property. On line 2, when it is referenced, since there is no variable `testFoo` in the local context, the program references the `testFoo` window object property.

## Global Variables and Function Declarations
When we declare global variables or functions, JavaScript adds them to the global object as properties.
```javascript
// Top of program
var total = 50;

function test() {
  console.log('This is a test.');
}

window.total; // 50
window.test(); // "This is a test."
```

This is almost identical to the above (non-declared) implementation. One key difference: **global properties and functions that have been declared can _not_ be deleted whereas those that haven't been declared as variables can.**

Example:
```javascript
var test1 = 'This is test 1.';
test2 = 'This is test 2.';

// Note: test1 and test2 are BOTH properties of the global object BUT only test2 can be deleted.
delete window.test1; // Nope... this doesn't work
delete window.test2; // Deleted!

// Same is true for functions
function testFunction() {
  console.log('This is a test function');
}

delete window.testFunction // Nope... again, this function was declared. Can't be deleted.
```

## A note on Node.js
In Node, the global object is `global`, not `window`.

Additionally, Node introduces **Module scope**. Variables in the module scope are those declared at the top level (not inside a function definition or object) of a node.js file. Module scoped variables are not added to the `global` object. Module-scoped variables are accessible anywhere within the file but not outside.
```javascript
var foo = 'foo';
// with `var` declaration, the variable is in the module scope
// and it is not added to the global object

bar = 'bar';
// without `var` declaration, the variable is in the global scope
// and added to the global object

console.log(global.foo);    // => undefined
console.log(global.bar);    // => bar
```

## Implicit and Explicit Function Execution Contexts
### Function Execution Contexts
* Every time a JavaScript function is invoked, it has access to an object called the **execution context** of that function.
* The execution context is accessible via the keyword `this`.
* A JavaScript function can be invoked in a variety of ways. Which object `this` refers to depends on how the function was invoked.

Two JavaScript Execution Contexts:
1. Implicit - An execution context that JS implicitly sets
2. Explicitly - An execution context that you set, explicitly.

### Implicit Function Execution Context
```javascript
function foo() {
  return 'this here is: ' + this;
}

foo(); // "this here is: [object Window]"
```
The above makes sense, as we know that the function declaration on line 1 binds `foo` to the global object.

Another example...
```javascript
var object = {
  foo: function () {
    return 'this here is: ' + this;
  },
};

object.foo(); // "this here is: [object Object]"

var bar = object.foo;
bar(); // "this here is: [object Window]"
```
The example here shows that the value of `this` is determined when the function **executes** not when it is defined.

## Explicit Function Execution Context
JavaScript lets us use the `apply` and `call` methods to change the functions's execution context at execution time.

Example:
```javascript
var a = 1;

var object = {
  a: 'hello',
  b: 'world',
};

function foo() {
  return this.a;
}

foo(); // 1 (context is global object)
foo.call(object); // "hello" (context is object)
```

Another example...
```javascript
var strings = {
  a: 'hello',
  b: 'world',
  foo: function () {
    return this.a + this.b;
  },
};

var numbers = {
  a: 1,
  b: 2,
};

strings.foo.call(numbers); // 3
```

* **The `call` method can also pass arguments to the function.** List the arguments one by one after the context object:
Example:
```javascript
someObject.someMethod.call(context, arg1, arg2, arg3, ...)
```
```javascript
var iPad = {
  name: 'iPad',
  price: 40000,
};
var kindle = {
  name: 'kindle',
  price: 30000,
};

function printLine(lineNumber, punctuation) {
  console.log(lineNumber + ': ' + this.name + ', ' + this.price / 100 + ' dollars' + punctuation);
}

printLine.call(iPad, 1, ';');        // => 1: iPad, 400 dollars;
printLine.call(kindle, 2, '.');      // => 2: kindle, 300 dollars.
```
In the above, `iPad` and `kindle` objects are the respective execution contexts. The remaining to arguments are the function arguments.

* **The `apply` method is identical to `call`, except that it uses an array to pass arguments.**
```javascript
someObject.someMethod.apply(context, [arg1, arg2, arg3, ...])
```
```javascript
printLine.apply(iPad, [1, '.'])  // 1: iPad, 400 dollars.
```

### `apply` vs `call` mnemonic
* **A**pply: **A**rrays as arguments
* **C**all: **c**ount the **c**ommas


## Hard Binding Functions with Context
* JavaScript has a keyword that allows you to bind a function to a context object _permanently_: `bind`
```javascript
var a = 'goodbye';

var object = {
  a: 'hello',
  b: 'world',
  foo: function() {
    return this.a + ' ' + this.b;
  },
};

var bar = object.foo;
bar();                                // "goodbye undefined"

var baz = object.foo.bind(object);
baz();                                // "hello world"

var object2 = {
  a: 'hi',
  b: 'there',
};

baz.call(object2);  // "hello world" - `this` is still `object`
```
* Unlike `call` or `apply`, bind doesn't execute a function. Instead, it creates and returns a new Function, and permanently binds it to a given object. 

Another example...
```javascript
var greetings = {
  morning: 'Good morning, ',
  afternoon: 'Good afternoon, ',
  evening: 'Good evening, ',

  greeting: function(name) {
    var currentHour = (new Date()).getHours();

    if (currentHour < 12) {
      console.log(this.morning + name);
    } else if (currentHour < 18) {
      console.log(this.afternoon + name);
    } else {
      console.log(this.evening + name);
    }
  },
};

var spanishWords = {
  morning: 'Buenos dias, ',
  afternoon: 'Buenas tardes, ',
  evening: 'Buena noches, ',
};

var spanishGreeter = greetings.greeting.bind(spanishWords);

spanishGreeter('Jose');
spanishGreeter('Juan');
```

## Dealing with Context Loss
Functions lose their contexts in three primary ways...

### 1. Methods losing context when taken out of an object
```javascript
var john = {
  firstName: 'John',
  lastName: 'Doe',
  greetings: function() {
    console.log('hello, ' + this.firstName + ' ' + this.lastName);
  },
};

var foo = john.greetings;
foo();

// => hello, undefined undefined
```
### 2. Internal Function losing Method Context
```javascript
var obj = {
  a: 'hello',
  b: 'world',
  foo: function() {
    function bar() {
      console.log(this.a + ' ' + this.b);
    }

    bar();
  },
};

obj.foo(); // => undefined undefined
```
In the above, `this` in the `bar` function call doesn't "bubble up" to the `obj` context. Instead, it defaults to global window object. Many developers view this as a mistake of language design. There are a few solutions that developers use to get around this.
  **Solution 1**: Preserve context with a local variable in the lexical scope
  A common approach is to `let self = this` and save `this` to a variable that you can reference in the internal function.
  ```javascript
  var obj = {
    a: 'hello',
    b: 'world',
    foo: function() {
      var self = this;

      function bar() {
        console.log(self.a + ' ' + self.b);
      }

      bar();
    }
  };

  obj.foo();
  >hello world
  ```

  **Solution 2**: Pass the context to internal functions
  You can also pass the context object to the function with `call` or `apply`
  ```javascript
  var obj = {
    a: 'hello',
    b: 'world',
    foo: function() {
      function bar() {
        console.log(this.a + ' ' + this.b);
      }

      bar.call(this);
    }
  };

  obj.foo();
  >hello world
  ```
  **Solution 3**: Bind the context with a function expression
  You can use `bind` when you define the function to give it a permanent context. 
  ```javascript
  var obj = {
    a: 'hello',
    b: 'world',
    foo: function() {
      var bar = function() {
        console.log(this.a + ' ' + this.b);
      }.bind(this);

      // some lines of code

      bar();

      // more lines of code

      bar();

      // ...
    }
  };

  obj.foo();
  >hello world
  >hello world
  ```
  **ES6 Solution**: Arrow functions do not define their own `this` value. 
  With arrow functions, `this` is scoped lexically.
  ```javascript
  const obj = {
    a: 'hello',
    b: 'world',
    foo: function() {
      const bar = () => {
        console.log(this.a + ' ' + this.b);
      }

      bar();
    }
  };

  obj.foo(); // 'hello world'
  ```
### Function as Argument losing surrounding context
```javascript
  var obj = {
    a: 'hello',
    b: 'world',
    foo: function() {
      [1, 2, 3].forEach(function(number) {
        console.log(String(number) + ' ' + this.a + ' ' + this.b);
      });
    },
  };

  obj.foo();

  // => 1 undefined undefined
  // => 2 undefined undefined
  // => 3 undefined undefined
```
As above, we can fix this by using a local variable in lexical scope, binding to `this`, or using an arrow function. Also, the `Array.prototype.forEach()` method takes an optional `thisArg` argument that explicitly passes a `this` object.
