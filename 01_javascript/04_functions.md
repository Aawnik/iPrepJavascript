## Function
- The reusable block of code designed to perform a specific task.
- Functions are "first-class citizens" in JavaScript, meaning they can be:
  - Assigned to variables
  - Passed as arguments to other functions
  - Returned from other functions
- Functions help organize code, promote reusability, and reduce redundancy

### Function Declaration
- Defined using the `function` keyword followed by a name
- Are hoisted to the top of their scope (can be called before declaration)
- Syntax: `function name(parameters) { /* code */ }`
- Example: `function greet(name) { return "Hello " + name; }`

### Function Expression
- Functions assigned to variables
- Not hoisted like function declarations
- Cannot be called before they are defined
- Syntax: `const name = function(parameters) { /* code */ };`
- Example: `const greet = function(name) { return "Hello " + name; };`

### Arrow Function
- Introduced in ES6 (ECMAScript 2015)
- Shorter syntax for writing function expressions
- Do not have their own `this`, `arguments`, `super`, or `new.target`
- Cannot be used as constructors
- Syntax: `const name = (parameters) => { /* code */ };`
- Shorter syntax for single expressions: `const add = (a, b) => a + b;`

### Anonymous Function
- Functions without names
- Often used as arguments to other functions or in IIFEs
- Syntax: `function() { /* code */ }`
- Example: `element.addEventListener('click', function() { alert('Clicked!'); });`

### Immediately Invoked Function Expression(IIFE)
- Functions that execute immediately after being defined
- Create a private scope to avoid polluting the global namespace
- Syntax: `(function() { /* code */ })();`
- Can also pass arguments: `(function(name) { console.log("Hello " + name); })("John");`

### Higher Order Functions
- Functions that operate on other functions by:
  - Taking functions as arguments
  - Returning functions as results
- Examples include `map()`, `filter()`, `reduce()`
- Create abstractions and enable functional programming techniques
- Example: `const twice = (f, x) => f(f(x));`

### callback Functions
- Functions passed as arguments to another function
- Executed after a specific event or operation completes
- Core pattern for asynchronous programming
- Enable non-blocking code execution
- Example: `fetchData(function(response) { console.log(response); });`

### default Parameters
- Allow parameters to have predetermined values if no value or `undefined` is passed
- Introduced in ES6
- Makes functions more flexible and robust
- Syntax: `function name(param1 = defaultValue) { /* code */ }`
- Example: `function greet(name = "Guest") { return "Hello " + name; }`

### Rest Parameters
- Collect all remaining arguments into an array
- Use the spread operator syntax `...`
- Allows functions to accept an indefinite number of arguments
- Syntax: `function name(...paramName) { /* code */ }`
- Example: `function sum(...numbers) { return numbers.reduce((a, b) => a + b, 0); }`

### Closures
- Functions that remember and access variables from their outer scope
- Persist even after the outer function has finished execution
- Enable data privacy, encapsulation, and stateful functions
- Example: 
    ```javascript
        function counter() {
          let count = 0;
          return function() {
            return ++count;
          };
        }
    ```
### Recursion
- A function that calls itself during its execution
- Requires a base case to prevent infinite recursion
- Useful for solving problems that can be broken down into similar sub-problems
- Often used for tree traversal, factorial calculation, and Fibonacci sequences
- Example: `function factorial(n) { return n <= 1 ? 1 : n * factorial(n - 1); }`

### Summary
    - Declaration vs Expression vs Arrow → different ways of defining functions.
    - IIFE, Callbacks, Higher-order functions → advanced usage.
    - Default & Rest params → modern ES6+ features.
    - Closures & Recursion → key interview topics

<hr><hr>

### Difference between function declaration and function expression?
- Function Declaration:
    - Defined using the function keyword followed by a name
    - Hoisted to the top of their scope
    - Can be called before they are defined in the code
    ```js
        greet("Alice"); // Works even though defined below
        function greet(name) {
          return "Hello, " + name;
        }
    ```
- Function Expression:
    - Functions assigned to variables
    - Not hoisted with their definition
    - Must be defined before they can be called
    ```js
        sayHello("Bob"); // Error: sayHello is not a function
        const sayHello = function(name) {
          return "Hello, " + name;
        };
    ```

### What are arrow functions?
- introduced in ES_6
    - Use the => syntax instead of the function keyword
    - Don't have their own bindings to this, arguments, super, or new.target
    - Automatically capture the this value from their surrounding context
    - Arrow functions simplify code and solve common issues with the this keyword in callbacks and event handlers.
### Can arrow functions be constructors?
- No, arrow functions cannot be used as constructors.
- Reasons:
    - They don't have their own this binding (they inherit from parent scope)
    - They don't have a prototype property
    - Attempting to use new with an arrow function throws a TypeError
    ```js
        const Person = (name) => {
          this.name = name; // 'this' refers to surrounding scope, not a new instance
        };

        const john = new Person("John"); // TypeError: Person is not a constructor
    ```
### What is function hoisting?
- JavaScript's behavior of moving declarations to the top of their containing scope during compilation
- Function declarations are fully hoisted with their definition
- Function expressions are not hoisted with their definition (only the variable declaration is hoisted)
- hoisting helps prevent unexpected behavior and errors in code execution order.
```js
    // This works because of hoisting
    sayHello("World");

    function sayHello(name) {
      console.log("Hello, " + name);
    }

    // This doesn't work - only the variable is hoisted, not the function assignment
    greet("Friend"); // Error: greet is not a function

    var greet = function(name) {
      console.log("Hi, " + name);
    };
```
### What is an IIFE (Immediately Invoked Function Expression)?
- A function expression that runs as soon as it's defined
- Creates a private scope, preventing variables from polluting the global namespace
- Common pattern before ES6 modules for encapsulation
  ```js
    (function() {
      // Private scope
      const secret = "I'm private!";
      console.log(secret); // Accessible here
    })();
    console.log(secret); // Error: secret is not defined
  ```
IIFEs help create private scopes and execute code immediately without creating global functions.
### Difference between parameters and arguments?
- Parameters:
  - Variable names listed in the function definition
  - Placeholders that receive values when the function is called
  - Defined at function declaration time
- Arguments:
  - The actual values passed to the function when it's called
  - Supply the data that parameters represent
  - Provided at function invocation time
  ```js
    // 'a' and 'b' are parameters
    function add(a, b) {
      return a + b;
    }
    // 5 and 3 are arguments
    add(5, 3);
  ```
### How do default parameters work?
- Allow parameters to have predetermined values if no value or undefined is passed
- Introduced in ES6
  - Makes functions more flexible and robust
  - Only triggered when argument is undefined (not null, false, 0, etc.)
  - Default parameters eliminate the need for manual parameter checking and simplify function definitions.
  ```js
    function greet(name = "Guest", greeting = "Hello") {
      return `${greeting}, ${name}!`;
    }

    greet();                  // "Hello, Guest!"
    greet("John");            // "Hello, John!"
    greet("Jane", "Welcome"); // "Welcome, Jane!"
    greet("Bob", undefined);  // "Hello, Bob!"
    greet("Alice", null);     // "null, Alice!" (null is a valid value, not undefined)
  ```
### Explain rest parameters (...args).
- Collects all remaining arguments into a single array parameter
- Uses the spread syntax (...) followed by a parameter name
- Must be the last parameter in a function definition
- Provides a cleaner alternative to using the arguments object
```js
  function sum(...numbers) {
    return numbers.reduce((total, num) => total + num, 0);
  }

  sum(1, 2, 3, 4);  // 10 (numbers = [1, 2, 3, 4])

  function logDetails(name, ...details) {
    console.log(`Name: ${name}`);
    console.log(`Details: ${details.join(', ')}`);
  }

  logDetails("Sarah", "Developer", "New York", 30);
  // Name: Sarah
  // Details: Developer, New York, 30
```
### What does `return` without value return?
- A function with `return;` or no return statement at all returns `undefined`.
- This behavior ensures that all functions return some value, allowing for consistent function composition and chaining.
```js
  function noReturn() {
    // No return statement
  }

  function emptyReturn() {
    return;
  }

  console.log(noReturn());    // undefined
  console.log(emptyReturn()); // undefined
```
### Can a function return another function?
Yes, a function can return another function.
  - This is a core concept of functional programming
  - Enables closures, currying, and higher-order functions
  - The returned function maintains access to variables in the outer function's scope.
  - Returning functions enables powerful programming patterns like function factories, currying, and composition that make code more flexible and reusable.
  ```js
    function multiplier(factor) {
      // Returns a function that multiplies by factor
      return function(number) {
        return number * factor;
      };
    }

    const double = multiplier(2);
    const triple = multiplier(3);

    console.log(double(5));  //10
    console.log(triple(5));  // 15
  ```

