# Objects and Methods

### Method Invocation vs. Function Invocation
JavaScript objects can contain _methods_ among their properties. Methods are functions with a _receiver_, which is the object that the method is called on. **If a call doesn't have an explicit receiver, it is a function.**

## `this`
The value of `this` is a reference to the object itself.

```javascript
const testObject = {
  widgets: 0,
  increaseWidgets: function() {
    this.widgets += 1;
  },
};

testObject; // { widgets: 0, increaseWidgets: [Function] }

testObject.increaseWidgets();
testObject; // { widgets: 1, increaseWidgets: [Functions] }
```

Methods are just object properties that have functions as values. You can define a method after the object is created.
```javascript
const testObject.run = function() {
  return "TestObject is running!";
};
```

## Mutating Objects
JavaScript Objects are mutable. Functions can alter the content of Objects passed in as arguments and those changes can be seen by any other code that references the Object.

### Primitive Values vs. Reference Values
**Primitive Values**
- String
- Number
- Boolean
- Undefined
- Null
- Symbol (in ES6)

Primitive values are stored on the _stack_.
The stack stores fast limited data.

**Reference Types**
- Objects
  - Arrays

Reference types are stored on the _heap_.
The heap is a large slow data store.

### Functions as Object Factories
Let's design a vehicle object.
```javascript
// All cars start out not moving, and sedans
// can accelerate about 8 miles per hour per second (mph/s).
const sedan = {
  speed: 0,
  rate: 8,

  accelerate() {
    this.speed += this.rate;
  },
};

const coupe = {
  speed: 0,
  rate: 12,
  accelerate() {
    this.speed += this.rate;
  },
};
```

Instead of copying the properties/methods for both objects, we can create a function that takes as an argument, the property values that change.
```javascript
function makeCar(mph) {
  return {
    speed: 0,
    rate: mph,
    accelerate() {
      this.speed += this.rate;
    }
  }
}
```

### Object Orientation
**Object Oriented Programming** is a programming pattern that uses objects as the basic building blocks of a program instead of local variables and funtions.
Objects...
* Organize related data and code together
* are useful when a program needs more than one instance of something
* become more useful as the code base size increases
