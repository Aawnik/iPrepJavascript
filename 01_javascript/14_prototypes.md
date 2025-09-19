## Prototypes in JavaScript
- Prototypes are a fundamental mechanism for inheritance in JavaScript. Unlike classical inheritance in languages like Java or C++, JavaScript uses prototypal inheritance where objects inherit directly from other objects.

What is Prototypal Inheritance?
Prototypal inheritance is the mechanism by which objects can inherit properties and methods from other objects. Every JavaScript object has a hidden link to another object called its "prototype".
```js
    function Animal(name) {
      this.name = name;
    }

    Animal.prototype.speak = function() {
      return `${this.name} makes a noise.`;
    };

    const dog = new Animal('Rex');
    console.log(dog.speak()); // "Rex makes a noise."
```
### Object.prototype vs __proto__
There are two key concepts to understand with prototypes:
- prototype property:
    - Exists on constructor functions
    - Contains properties/methods that will be inherited by instances
    - Is used as the prototype for objects created with the constructor
- __proto__:
    - Exists on all objects
    - References the prototype of the object
    - Creates the prototype chain for inheritance
```js
    function Person(name) {
      this.name = name;
    }

    Person.prototype.greet = function() {
      return `Hello, I'm ${this.name}`;
    };

    const john = new Person('John');

    console.log(Person.prototype); // {greet: ƒ, constructor: ƒ}
    console.log(john.__proto__); // {greet: ƒ, constructor: ƒ}
    console.log(john.__proto__ === Person.prototype); // true
```
### The Prototype Chain
When you try to access a property on an object, JavaScript:
    - Checks if the property exists on the object itself
    - If not, it checks the object's prototype
    - If not found, it checks the prototype's prototype...
    - Continues until it reaches Object.prototype
    - Returns undefined if not found anywhere
```js
    const arr = [1, 2, 3];
    console.log(arr.toString()); // "1,2,3" - method from Array.prototype
    console.log(arr.hasOwnProperty('length')); // true - method from Object.prototype
```

### Adding Methods to the prototype
```js
    // Adding methods to existing prototype
    Array.prototype.first = function() {
      return this[0];
    };
    const numbers = [10, 20, 30];
    console.log(numbers.first()); // 10

    // Creating custom constructor with prototype methods
    function Calculator() {
      this.result = 0;
    }

    Calculator.prototype.add = function(num) {
      this.result += num;
      return this;
    };

    Calculator.prototype.subtract = function(num) {
      this.result -= num;
      return this;
    };

    Calculator.prototype.getResult = function() {
      return this.result;
    };
    const calc = new Calculator();
    console.log(calc.add(5).subtract(2).getResult()); // 3
```
### Prototype Inheritance
```js
    // Parent constructor
    function Animal(name) {
      this.name = name;
    }

    Animal.prototype.eat = function() {
      return `${this.name} is eating.`;
    };

    // Child constructor
    function Dog(name, breed) {
      Animal.call(this, name); // Call parent constructor
      this.breed = breed;
    }

    // Set up inheritance
    Dog.prototype = Object.create(Animal.prototype);
    Dog.prototype.constructor = Dog; // Reset constructor

    // Add methods to Dog prototype
    Dog.prototype.bark = function() {
      return 'Woof!';
    };

    // Override parent method
    Dog.prototype.eat = function() {
      return `${this.name} is eating like a dog.`;
    };

    const rex = new Dog('Rex', 'German Shepherd');
    console.log(rex.name); // "Rex"
    console.log(rex.breed); // "German Shepherd"
    console.log(rex.eat()); // "Rex is eating like a dog."
    console.log(rex.bark()); // "Woof!"
```

### Object.create()
- cleaner way to create the objects with specific prototypes
```js
const personProto = {
  greet() {
    return `Hello, my name is ${this.name}`;
  },
  setName(name) {
    this.name = name;
  }
};

const john = Object.create(personProto);
john.setName('John');
console.log(john.greet()); // "Hello, my name is John"
```
### Checking the prototype chain
```js
function isPrototypeOf(object, prototype) {
  return prototype.isPrototypeOf(object);
}

const arr = [1, 2, 3];
console.log(isPrototypeOf(arr, Array.prototype)); // true
console.log(isPrototypeOf(arr, Object.prototype)); // true
console.log(isPrototypeOf(arr, String.prototype)); // false
```
### Prototype Vs Classes
- ES6 introduced class syntax, which is syntactic sugar over prototype-based inheritance:
```js
// Class syntax (ES6+)
class Animal {
  constructor(name) {
    this.name = name;
  }
  
  speak() {
    return `${this.name} makes a noise.`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }
  
  speak() {
    return `${this.name} barks.`;
  }
}

// Under the hood, still using prototypes!
const rex = new Dog('Rex', 'German Shepherd');
console.log(rex.speak()); // "Rex barks."
```

## Common Pitfalls
### Modifying built-in Prototype
```js
// Generally considered bad practice
Array.prototype.first = function() { return this[0]; };

// Can lead to unexpected behavior in libraries or future JS versions
```
### `this` context in prototype methods
```js
function Counter() {
  this.count = 0;
}

Counter.prototype.increment = function() {
  this.count++;
};

const counter = new Counter();
const increment = counter.increment;
// increment(); // Error: this is undefined (or window in non-strict mode)

// Fix: Bind the method
const boundIncrement = counter.increment.bind(counter);
boundIncrement(); // Works correctly
```
- NOTE:_Prototypes are a powerful feature of JavaScript that enable efficient code reuse and proper encapsulation when used correctly.


<hr><hr>

### 1. What is prototypal inheritance?
Prototypal inheritance is JavaScript's mechanism where objects can inherit properties and methods directly from other objects, rather than from classes as in classical inheritance.
```js
function Animal(name) {
  this.name = name;
}

Animal.prototype.speak = function() {
  return `${this.name} makes a noise.`;
};

function Dog(name) {
  Animal.call(this, name); // Inherit constructor properties
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

// Add/override methods
Dog.prototype.speak = function() {
  return `${this.name} barks.`;
};

const dog = new Dog('Rex');
console.log(dog.speak()); // "Rex barks."
```
NOTE:_classical inheritance (classes inheriting from classes), JavaScript uses objects inheriting from objects. This creates a more flexible inheritance model where behaviors can be inherited at runtime through prototype links rather than being defined statically at compile time.

### 2. Difference between __proto__ and prototype?
- prototype:
    - Is a property of constructor functions only
    - Contains properties and methods that will be inherited by instances
    - Forms the blueprint for objects created with that constructor
- `__proto__` (or Object.getPrototypeOf()):
    - Is a property of all objects
    - References the prototype object of the constructor that created the object
    - Creates the actual link in the prototype chain
    - Is the internal property that JavaScript uses for inheritance lookups
```js
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

const john = new Person('John');

console.log(Person.prototype); // {greet: ƒ, constructor: ƒ}
console.log(john.__proto__);   // {greet: ƒ, constructor: ƒ} (same object)
console.log(john.__proto__ === Person.prototype); // true
```
Reason: The distinction exists because JavaScript separates object creation (via constructors) from inheritance (via prototypes). The prototype property defines what will be inherited, while __proto__ establishes the inheritance link in created objects.

### 3. How do you add methods to a prototype?
You can add methods to a prototype in several ways:
```js
// Method 1: Direct assignment to prototype
function Person(name) {
  this.name = name;
}

Person.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};

Person.prototype.sayGoodbye = function() {
  return `Goodbye from ${this.name}`;
};

// Method 2: Multiple assignments using Object.assign
Object.assign(Person.prototype, {
  introduce: function() {
    return `My name is ${this.name}`;
  },
  celebrateBirthday: function() {
    return `${this.name} is celebrating!`;
  }
});

// Method 3: Setting prototype all at once (less common)
Person.prototype = {
  constructor: Person, // Must restore this!
  greet: function() {
    return `Hello, I'm ${this.name}`;
  },
  sayGoodbye: function() {
    return `Goodbye from ${this.name}`;
  }
};

const john = new Person('John');
console.log(john.greet()); // "Hello, I'm John"
```
Reason: Adding methods to prototypes is more memory-efficient than adding them to individual instances. When methods are added to the prototype, all instances share the same function definition rather than each having its own copy.

### 4. What is the prototype chain?
The prototype chain is the mechanism JavaScript uses to look up properties and methods on objects. When you access a property or method, JavaScript:
    - Checks if the property exists directly on the object
    - If not, it checks the object's prototype (__proto__)
    - If not found, it checks the prototype's prototype
    - This continues until it reaches Object.prototype (the end of the chain)
    - Returns undefined if the property isn't found anywhere in the chain
```js
function Animal() {}
Animal.prototype.eat = function() { return 'Eating...'; };

function Dog() {}
// Set up inheritance chain: Dog -> Animal -> Object
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;
Dog.prototype.bark = function() { return 'Woof!'; };

const rex = new Dog();

console.log(rex.bark());  // 'Woof!' - Found on Dog.prototype
console.log(rex.eat());   // 'Eating...' - Found on Animal.prototype
console.log(rex.toString()); // '[object Object]' - Found on Object.prototype
console.log(rex.fly());   // undefined - Not found in prototype chain

// Visualizing the chain:
console.log(rex.__proto__ === Dog.prototype);  // true
console.log(rex.__proto__.__proto__ === Animal.prototype);  // true
console.log(rex.__proto__.__proto__.__proto__ === Object.prototype);  // true
console.log(rex.__proto__.__proto__.__proto__.__proto__ === null);  // true - End of chain
```
Reason: The prototype chain enables inheritance and property sharing across objects. It's an elegant way to reuse code without duplicating it on every object instance, forming the foundation of JavaScript's object-oriented capabilities.

5. How do you override prototype methods?
You can override prototype methods by simply defining a method with the same name directly on an object or on a prototype lower in the chain:
```js
// Parent constructor
function Animal(name) {
  this.name = name;
}

// Add method to parent prototype
Animal.prototype.speak = function() {
  return `${this.name} makes a noise.`;
};

// Child constructor
function Dog(name, breed) {
  Animal.call(this, name);
  this.breed = breed;
}

// Set up inheritance
Dog.prototype = Object.create(Animal.prototype);
Dog.prototype.constructor = Dog;

// Override the speak method
Dog.prototype.speak = function() {
  return `${this.name} barks loudly!`;
};

// Create instances
const generic = new Animal('Generic Animal');
const dog = new Dog('Rex', 'German Shepherd');

console.log(generic.speak()); // "Generic Animal makes a noise."
console.log(dog.speak());     // "Rex barks loudly!"

// Instance-specific override
dog.speak = function() {
  return `${this.name} the ${this.breed} says woof!`;
};

console.log(dog.speak()); // "Rex the German Shepherd says woof!"
```
Reason: Method overriding allows child objects to provide specific implementations of methods defined in their prototype chain. This enables polymorphism, where the same method name can have different behaviors depending on the object's type. JavaScript resolves method calls by searching the prototype chain from bottom to top, so the most specific implementation (closest to the instance) takes precedence.



