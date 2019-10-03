# JavaScript Object Creation Pattersn

## Factory Functions
The **_factory object creation pattern_** defines a function that returns an object based on a pre-defined template.

```javascript
function createPerson(firstName, lastName) {
  var person = {};
  person.firstName = firstName;
  person.lastName = lastName || '';
  person.fullName = function() {
    return (this.firstName + ' ' + this.lastName).trim();
  };

  return person;
}

var john = createPerson('John', 'Doe');
var jane = createPerson('Jane');

john.fullName();        // "John Doe"
jane.fullName();        // "Jane"
```

**Advantages**:
Allows us to easily replicate objects based on predefined template or pattern.

**Disadvantages**:
* Each object has a full copy of all methods, which can become redundant
* There isn't a way to inspect an object and determine whether it was created from a factory function. Thus, it is difficult to determine a "type".

## Constructor Pattern
```javascript
// constructor function
function Person(firstName, lastName) {
  this.firstName = firstName;
  this.lastName = lastName || '';
  this.fullName = function() {
    return (this.firstName + ' ' + this.lastName).trim();
  };
}

var john = new Person('John', 'Doe');
var jane = new Person('Jane');

john.fullName();              // "John Doe"
jane.fullName();              // "Jane"

john.constructor;             // function Person(..)
jane.constructor;             // function Person(..)

john instanceof Person;       // true
jane instanceof Person;       // true
```
In the example above, we call `Person` a constructor function because it is intended to be called with the `new` operator.

When we call a function with the `new` operator, the following happens:
1. A new object is created
2. `this` in the function is set to point to the new object
3. The code in the function is executed
4. `this` is returned if the constructor doesn't explicity return an object

**Note:** We typically capitalize constructor functions but this is just convention.

### Objects' Prototypes
* Every JavaScript object has a special property, `__proto__` (pronounced "dunder proto," with "dunder" short for "double underscore").
* When we use the `Object.create` method to create an object, it sets the `__proto__` property of the created object to the object that was passed into `Object.create` as an argument.
```javascript
var foo = {
  a: 1,
  b: 2,
};

var bar = Object.create(foo);
foo.isPrototypeOf(bar) === true; // true       
```
In the case above, we say that `foo` is the _prototype object_ for `bar`.

We can use `Object.getPrototypeOf(obj)`to get the prototype of `obj`.
We can use `obj.isPrototypeOf(foo)` to check if `obj` is a prototype of `foo`.

### Prototype Chain
We can use `Object.create` to create objects to form a prototype chain.
```javascript
var foo = {
  a: 1,
  b: 2,
};

var bar = Object.create(foo);
var baz = Object.create(bar);
var qux = Object.create(baz);

Object.getPrototypeOf(qux) === baz;       // true
Object.getPrototypeOf(baz) === bar;       // true
Object.getPrototypeOf(bar) === foo;       // true

foo.isPrototypeOf(qux);                   // true - because foo is on qux's prototype chain
```
**Note:** The `Object.prototype` object is at the end of the prototype chain for all JS objects. If you don't create an object from a prototype, it's prototype is the `Object.prototype` object.
```javascript
var foo = {
  a: 1,
  b: 2,
};                                // created with object literal

Object.getPrototypeOf(foo) === Object.prototype;      // true
```

### Prototypal Inheritance
When we try to access a property or a method on an object, JS searches not only teh object itself, but all of the objects on its prototype chain, until the value is found or until the end of the prototype chain is reached.

This gives us the ability to store an object's data and behaviors not just in the object itself, but anywhere on the prototype chain.
```javascript
var dog = {
  say: function() {
    console.log(this.name + ' says Woof!');
  },

  run: function() {
    console.log(this.name + ' runs away.');
  },
};

var fido = Object.create(dog);
fido.name = 'Fido';
fido.say();             // => Fido says Woof!
fido.run();             // => Fido runs away.

var spot = Object.create(dog);
spot.name = 'Spot';
spot.say();             // => Spot says Woof!
spot.run();             // => Spot runs away.
```
The above example shows the benefits of prototypal inheritance:
* We can create dogs much more easily with the `dog` prototype, and don't have to duplicate say and run on every single dog object.
* If we need to add/remove/update behavior to apply to all dogs, we can just modify the prototype object, and all dogs will pick up the changed behavior automatically.

#### Inheritance vs. Behavior Delegation
From a top down / design time point of view, the objects on the bottom of the prototype chain "inherited" the properties and behaviors of all the upstream objects on the prototype chain; from a bottom up / run time point of view, objects on the bottom of the prototype chain can "delegate" requests to the upstream objects to be handled. Hence this design pattern is also called **Behavior Delegation**.

#### Polymorphism
Objects created from prototypes can override shared behaviors by defining the same methods locally. This allows us to implement _polymorphism_ or giving different behaviors to methods with the same name.
```javascript
var dog = {
  say: function() {
    console.log(this.name + ' says Woof!');
  },
};

var fido = Object.create(dog);
fido.name = 'Fido';
fido.say = function() {
  console.log(this.name + ' says Woof Woof!');
};

fido.say();             // => Fido says Woof Woof!
var spot = Object.create(dog);
spot.name = 'Spot';
spot.say();             // => Spot says Woof!
```

#### object.hasOwnProperty
Since `obj.prop` will search all the way up the prototype chain for the existence of `prop`, it is not reliable to test if an object has a property using `obj.prop !== undefined`. Thus, we should use the `hasownProperty` and `Object.getOwnPropertyNames` methods.
```javascript
var foo = {
  a: 1,
};

var bar = Object.create(foo);
bar.a = 1;
bar.b = 2;
bar.hasOwnProperty('a');            // true
Object.getOwnPropertyNames(bar);    // ["a", "b"]

delete bar.a;
bar.hasOwnProperty('a');            // false
Object.getOwnPropertyNames(bar);    // ["b"]

bar.hasOwnProperty('c');            // false
```

## The Prototype Pattern

### Function Prototypes vs. Object Prototypes
In JavaScript, every function has a special `prototype` property. It is assigned, by default,an object that instances of the constructor function can delegate to. This `prototype` property is only useful when we use the function as a constructor, in which case all objects that it constructs will have this object set as their prototype.

Here's an example of how/when this could be useful:
```javascript
var Dog = function() {};

Dog.prototype.say = function() {
  console.log(this.name + ' says Woof!');
}

Dog.prototype.run = function() {
  console.log(this.name + ' runs away.');
}

var fido = new Dog();
fido.name = 'Fido';
fido.say();             // => Fido says Woof!
fido.run();             // => Fido runs away.

var spot = new Dog();
spot.name = 'Spot';
spot.say();             // => Spot says Woof!
spot.run();             // => Spot runs away.
```
This approach to defining shared behavior is called **the Prototype Pattern** of object creation.
