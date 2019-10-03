# Closures and Function Scope

* **Closures allow us to define private data that persists for the function's lifetime.**

## Lexical Scoping:
All functions, regardless of syntax, obey the same lexical scoping rules:
1. They can access any variables defined within it.
2. They can access any variables that were in scope based on the context where the function was defined.

## Higher Order Functions
Higher order functions are functions that either accept a function as an argument or return a function.

## Garbage Collection
JavaScript is a garbage-collected language. That means the language runtime handles _memory deallocation_, not the developer.

## Immediately Invoked Function Expressions
* An _immediately invoked function expression_ (or IIFE) is a function that we define and invoke at the same time.
```javascript
(function() {
  console.log('hello');
})();    
```
* IIFEs can take arguments.
```javascript
(function(a) {
  return a + 1;
})(2);                    // 3
```
* We can omit the surrounding parentheses when the function expression does not begin a the beginning of a line (think function expressions and nested functions)
```javascript
var foo = function() {
  return function() {
    return 10;
  }();
}();

console.log(foo);       // => 10
```

### Benefits of IIFEs
* Each IIFE creates it's own function scope, when making changes in a large codebase, you can be assure that you will not override any other variables or functions in the codebase.
* You can use an IIFE to return a function with access to private data.
```javascript
var generateStudentId = (function() {
  var studentId = 0;

  return function() {
    studentId += 1;
    return studentId;
  };
})();
```
* We can also use IIFEs to return an object that has access to private data!
```javascript
var inventory = (function() {
  var stocks = [];
  function isValid(newStock) {
    return stocks.every(function(stock) {
      return newStock.name !== stock.name;
    }); 
  }

  return { 
    stockCounts: function() {
      stocks.forEach(function(stock) {
        console.log(stock.name + ': ' + String(stock.count));
      });
    },
    addStock: function(newStock) {
      if (isValid(newStock)) { stocks.push(newStock) }
    },
  };
})();
```
In the above example, inventory references an object that has `stockCounts` and `addStock` methods. These methods, themselves, can access the variable `stocks` and the function `isValid`. These however, are _not_ exposed to `inventory`.
