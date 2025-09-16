## Classes in JavaScript
Classes were introduced in ES6 (ECMAScript 2015) to provide a clearer, more object-oriented syntax for creating objects and dealing with inheritance. Under the hood, JavaScript classes are primarily syntactic sugar over JavaScript's existing prototype-based inheritance.
### Basic Class Syntax
```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
  
  greet() {
    return `Hello, my name is ${this.name}`;
  }
  
  getAge() {
    return this.age;
  }
}

const john = new Person("John", 30);
console.log(john.greet()); // "Hello, my name is John"
console.log(john.getAge()); // 30
```
### Constructor Method
The constructor method is special:
    - It's called automatically when a new instance is created
    - It's used to initialize object properties
    - If you don't define a constructor, a default empty one is created
```js
class Product {
  constructor(name, price) {
    this.name = name;
    this.price = price;
    this.timestamp = Date.now(); // Automatically track creation time
  }
}
```
### Class Methods
```js
class Calculator {
  add(a, b) {
    return a + b;
  }
  
  subtract(a, b) {
    return a - b;
  }
  
  multiply(a, b) {
    return a * b;
  }
  
  divide(a, b) {
    if (b === 0) throw new Error("Cannot divide by zero");
    return a / b;
  }
}

const calc = new Calculator();
console.log(calc.add(5, 3)); // 8
```

### Static Methods and Properties
Static members belong to the class itself, not to instances:
```js
class MathUtils {
  static PI = 3.14159;
  
  static square(x) {
    return x * x;
  }
  
  static cube(x) {
    return x * x * x;
  }
}

console.log(MathUtils.PI); // 3.14159
console.log(MathUtils.square(4)); // 16
```
### Class Inheritance
The extends keyword creates a subclass:
```js
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
    super(name); // Call parent constructor
    this.breed = breed;
  }
  
  speak() {
    return `${this.name} barks!`;
  }
  
  getBreed() {
    return this.breed;
  }
}

const dog = new Dog("Rex", "German Shepherd");
console.log(dog.speak()); // "Rex barks!"
console.log(dog.getBreed()); // "German Shepherd"
```
### The super Keyword
super has two main uses:
    - In constructors: calls the parent class constructor
    - In methods: calls the parent class method
```js
class Vehicle {
  constructor(type, speed) {
    this.type = type;
    this.speed = speed;
  }
  
  getInfo() {
    return `Type: ${this.type}, Speed: ${this.speed}mph`;
  }
}

class Car extends Vehicle {
  constructor(make, model, speed) {
    super('car', speed); // Call parent constructor
    this.make = make;
    this.model = model;
  }
  
  getInfo() {
    return `${super.getInfo()}, Make: ${this.make}, Model: ${this.model}`;
  }
}

const myCar = new Car('Honda', 'Civic', 120);
console.log(myCar.getInfo()); 
// "Type: car, Speed: 120mph, Make: Honda, Model: Civic"
```

### Getters & Setters
```js
class Temperature {
  constructor(celsius) {
    this._celsius = celsius;
  }
  
  get celsius() {
    return this._celsius;
  }
  
  set celsius(value) {
    if (value < -273.15) {
      throw new Error("Temperature below absolute zero is not possible");
    }
    this._celsius = value;
  }
  
  get fahrenheit() {
    return this._celsius * 9/5 + 32;
  }
  
  set fahrenheit(value) {
    this.celsius = (value - 32) * 5/9;
  }
}

const temp = new Temperature(25);
console.log(temp.celsius); // 25
console.log(temp.fahrenheit); // 77

temp.celsius = 30;
console.log(temp.fahrenheit); // 86

temp.fahrenheit = 68;
console.log(temp.celsius); // 20
```
### Private Class Fields
Modern JavaScript (ES2022) supports truly private fields using the # prefix:
```js
class BankAccount {
  #balance = 0; // Private field
  #pin;
  
  constructor(accountHolder, initialBalance, pin) {
    this.accountHolder = accountHolder; // Public field
    this.#balance = initialBalance;
    this.#pin = pin;
  }
  
  deposit(amount) {
    if (amount > 0) {
      this.#balance += amount;
      return true;
    }
    return false;
  }
  
  withdraw(amount, pin) {
    if (pin !== this.#pin) {
      console.log("Invalid PIN");
      return false;
    }
    
    if (amount > 0 && amount <= this.#balance) {
      this.#balance -= amount;
      return true;
    }
    return false;
  }
  
  getBalance(pin) {
    return pin === this.#pin ? this.#balance : "Access denied";
  }
}

const account = new BankAccount("John Doe", 1000, 1234);
console.log(account.getBalance(1234)); // 1000
account.deposit(500);
console.log(account.getBalance(1234)); // 1500
account.withdraw(200, 1234);
console.log(account.getBalance(1234)); // 1300
console.log(account.getBalance(5678)); // "Access denied"
console.log(account.#balance); // SyntaxError: Private field
```

### Classes vs Prototype Inheritance
Classes are essentially syntactic sugar over JavaScript's prototype-based inheritance:
```js
// Class syntax
class Person {
  constructor(name) {
    this.name = name;
  }
  
  greet() {
    return `Hello, I'm ${this.name}`;
  }
}

// Equivalent prototype-based approach
function PersonES5(name) {
  this.name = name;
}

PersonES5.prototype.greet = function() {
  return `Hello, I'm ${this.name}`;
};
```
### Key Differences from Other Languages
- JavaScript classes are not hoisted (unlike function declarations)
- All code inside classes runs in strict mode automatically
- Class methods are non-enumerable by default
- Constructor must be called with new

Practical Example: Building a Shopping Cart
```js
class Product {
  constructor(id, name, price) {
    this.id = id;
    this.name = name;
    this.price = price;
  }
  
  getFormattedPrice() {
    return `$${this.price.toFixed(2)}`;
  }
}

class CartItem {
  constructor(product, quantity = 1) {
    this.product = product;
    this.quantity = quantity;
  }
  
  getSubtotal() {
    return this.product.price * this.quantity;
  }
}

class ShoppingCart {
  #items = [];
  
  addItem(product, quantity = 1) {
    const existingItem = this.#items.find(item => item.product.id === product.id);
    
    if (existingItem) {
      existingItem.quantity += quantity;
    } else {
      this.#items.push(new CartItem(product, quantity));
    }
  }
  
  removeItem(productId) {
    this.#items = this.#items.filter(item => item.product.id !== productId);
  }
  
  updateQuantity(productId, quantity) {
    const item = this.#items.find(item => item.product.id === productId);
    if (item) {
      if (quantity > 0) {
        item.quantity = quantity;
      } else {
        this.removeItem(productId);
      }
    }
  }
  
getItems() {
    return [...this.#items];
  }
  
  getTotal() {
    return this.#items.reduce((total, item) => total + item.getSubtotal(), 0);
  }
  
  clear() {
    this.#items = [];
  }
}

// Usage
const laptop = new Product(1, "Laptop", 999.99);
const phone = new Product(2, "Smartphone", 699.99);
const headphones = new Product(3, "Headphones", 149.99);

const cart = new ShoppingCart();
cart.addItem(laptop);
cart.addItem(phone, 2);
cart.addItem(headphones);

console.log("Items in cart:", cart.getItems().length); // 3
console.log("Total:", cart.getTotal().toFixed(2)); // 2549.96
```

<hr><hr>

### How do you define a class in JavaScript?
```js
class ClassName {
  constructor(param1, param2) {
    this.param1 = param1;
    this.param2 = param2;
  }
  
  methodName() {
    // method implementation
  }
}
```
- Reason: Classes in JavaScript are defined using the class keyword followed by the name of the class. The class body is enclosed in curly braces and contains the class constructor and methods. Classes were introduced in ES6 to provide a cleaner syntax for creating objects and implementing inheritance, though they're primarily syntactic sugar over JavaScript's prototype-based inheritance.

### What is a constructor method?
```js
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }
}
```
- Reason: The constructor is a special method that gets invoked automatically when a new instance of a class is created with the new keyword. It's used to initialize object properties and perform setup operations. If you don't explicitly define a constructor, JavaScript provides a default empty constructor. The constructor must be named "constructor" and you can only have one constructor per class.

### How do you extend a class?
```js
class Animal {
  constructor(name) {
    this.name = name;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);  // Call parent constructor
    this.breed = breed;
  }
}
```
Reason: You extend a class using the extends keyword, which creates a subclass that inherits properties and methods from its parent class. This enables code reuse and establishes an "is-a" relationship between classes. The super() call in the constructor is mandatory if the subclass has its own constructor, and it must be called before using this in the subclass constructor.

### What is method overriding in classes?
```js
class Animal {
  speak() {
    return "Animal makes a sound";
  }
}

class Dog extends Animal {
  speak() {
    return "Dog barks";  // Overrides the parent method
  }
}
```
Reason: Method overriding occurs when a child class defines a method with the same name as a method in the parent class. The child's implementation "overrides" the parent's implementation when called on an instance of the child class. This enables polymorphism, where different objects can respond to the same method call in ways specific to their type. You can still access the parent's method using super.methodName().

### Difference between class and function constructor?
classSyntax
  ```js
    class Person {
      constructor(name) {
        this.name = name;
      }

      greet() {
        return `Hello, I'm ${this.name}`;
      }
    }
  ```
Function Constructor Syntax
  ```js
    function Person(name) {
      this.name = name;
    }

    Person.prototype.greet = function() {
      return `Hello, I'm ${this.name}`;
    };
  ```
Reasons for differences:
  - Syntax: Classes offer a cleaner, more intuitive syntax compared to function constructors.
  - Hoisting: Classes are not hoisted, while function constructors are. You must declare a class before using it.
  - Strict mode: Code inside a class body runs in strict mode automatically.
  - Method definition: In classes, methods are defined directly in the class body, while in function constructors, methods are typically added to the prototype.
  - Inheritance: Classes use the extends keyword for inheritance, which is more straightforward than the prototype-based inheritance of function constructors.
  - Constructor invocation: Classes must be called with the new keyword, while function constructors technically can be called without it (though that creates problems).
  - Under the hood: Despite their differences, classes are primarily syntactic sugar over JavaScript's prototype-based inheritance model.

