# JavaScript Module Pattern (Pre-ES6)

* To encapsulate and export properties and methods, we can use the _module pattern_.
* **Note**: ES6 introduces native modules to JavaScript but they are not supported in the browser.
  * To use ES6 Modules in browser, one must use Babel to compile and use Webpack as a module loader.


General Pattern: Wrap the entire block in a function that returns an object literal and then _immediately invoke_ it.

Example:
```javascript
const UICtrl = (function() {
  let text = 'Hello World';
  
  const changeText = function() {
    const element = document.querySelector('h1');
    element.textContent = text;
  }

  return {
    callChangeText: function() {
      changeText();
    }
  }

})()

UICtrl.callChangeText();
```

In the above example, because variables `text` and `changeText` are scoped at the level of the IIFE, they cannot be accessed outside of the function. The function expression, however, is immediately invoked nad returns the object literal `{ callChangeText: function() { changeText() } }`. This variable `UICtrl` points to this object. Thus we can call object method, `callChangeText()` on `UICtrl`.

## The Singleton Pattern (A manifestation of the Module Pattern)
* A singleton pattern is an immediate anonymous function and it can only return one instance of an object. Repeated calls to the constructor will just return the same instance.




