# Object-Oriented JavaScript (Classes)

```javascript
class Dog {
  constructor(name) {
    this._name = name;
    this._behavior = 0;
  }

  get name() {
    return this._name;
  }
  get behavior() {
    return this._behavior;
  }   

  incrementBehavior() {
    this._behavior ++;
  }
}

const halley = new Dog('Halley');
console.log(halley.name); // Print name value to console
console.log(halley.behavior); // Print behavior value to console
halley.incrementBehavior(); // Add one to behavior
console.log(halley.name); // Print name value to console
console.log(halley.behavior); // Print behavior value to console
```

## The Constructor
```javascript
class Dog {
  constructor(name) {
    this._name = name;
    this._behavior = 0;
  }
}
```
* JavaScript calls the `constructor()` method every time it creates a new _instance_ of a class.
* `Dog` is the name of our class. By convention, it is capitalized and camelcase.
* We use `this` to refer to the instance of the class.
* Note: JavaScript does not support private or protected properties. Instead, we use the conventional undescore to indicate that this property should not be accessed directly.

## Instances
* An instance is an object that contains the property names and methods of a class, but with unique property values.

## Getters
* _Getters_ are methods that get and return internal properties of an object.
* Syntactically, we don't need parentheses when callling a getter. Thus, it looks just like we're accessing a property.
* Moreover, getters can perform an action on the data when getting a property.
* Getters can return different values using conditionals
* This is polymorphism!
```javascript
class Person {
  constructor(firstName, lastName) {
    this._firstName = firstName;
    this._lastName = lastName;
  }
  
  get fullName() {
    if (this._firstName && this._lastName){
      return `${this._firstName} ${this._lastName}`;
    } else {
      return 'Missing a first name or a last name.';
    }
  }
}
```

## Setters
* We can also create setter methods, which reassign values of existing properties within an object.
* With setters, we can validate incoming parameters using conditionals.
* As with getters, we don't need parentheses when calling setters.
```javascript
class Person {
  constructor(age) {
    this._age = age;
  }
  
  set age(newAge){
    if (typeof newAge === 'number'){
      this._age = newAge;
    } else {
      console.log('You must assign a number to age');
    }
  }
};

person.age = 40;
console.log(person._age); // Logs: 40
person.age = '40'; // Logs: You must assign a number to age
```

## Inheritance
* When multiple classes share properties or methods, they become candidates for _inheritance_.
* With inheritance, you create a _parent class_ (also known as a _superclass_).
* Parent classes have properties and methods that multiple _child_ classes can share.
```javascript
class Animal {
  constructor(name) {
    this._name = name;
    this._behavior = 0;
  }

  get name() {
    return this._name;
  }

  get behavior() {
    return this._behavior;
  }   

  incrementBehavior() {
    this._behavior++;
  }
} 

class Cat extends Animal {
  constructor(name, usesLitter) {
    super(name);
    this._usesLitter = usesLitter;
  }
}
```

* The `extends` keyword makes the methods of the animal class available inside the cat class. In other words, when you see `super` think: "go to my superclass, find a method with the exact same name, then copy and paste that code here in my place. For any parameters, pass in the parameters passed to `super`"

## Static Methods
* These are like Ruby class methods. These methods are called on the _class_ not the instance.
* Take the `Date` class, for example — you can both create `Date` instances to represent whatever date you want, and call static methods, like Date.now() which returns the current date, directly from the class. The .now() method is static, so you can call it directly from the class, but not from an instance of the class.

```javascript
class Animal {
  constructor(name) {
    this._name = name;
    this._behavior = 0;
  }

  static generateName() {
    const names = ['Angel', 'Spike', 'Buffy', 'Willow', 'Tara'];
    const randomNumber = Math.floor(Math.random()*5);
    return names[randomNumber];
  }
} 

console.log(Animal.generateName()); // returns a name

const tyson = new Animal('Tyson'); 
tyson.generateName(); // TypeError
```

