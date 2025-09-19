## Scope & Hoisting in Javascript
Scope and hoisting are fundamental JavaScript concepts that influence how variables and functions are accessible and initialized in your code.

### Scope in JavaScript
Scope determines the visibility and accessibility of variables and functions in different parts of your code.
- Global Scope
    - Variables declared outside any function or block have global scope and can be accessed from anywhere in the code.
    ```js
        // Global scope
        const globalVar = "I'm global";
        function testFunction() {
          console.log(globalVar); // Accessible here
        }

        console.log(globalVar); // Accessible here too
- Local/Function Scope
    - Variables declared inside a function are only accessible within that function.
    ```js
        function myFunction() {
          // Function scope
          const localVar = "I'm local";
          console.log(localVar); // Accessible
        }

        myFunction();
        console.log(localVar); // ReferenceError: localVar is not defined
- Block Scope
    - Variables declared with let and const are block-scoped, meaning they're only accessible within the block (enclosed by {}) where they're defined.
    ```js
        if (true) {
          // Block scope
          let blockVar = "I'm in a block";
          const anotherBlockVar = "I'm also in a block";
          var notBlockScoped = "I'm not block scoped";

          console.log(blockVar); // Accessible
        }

        console.log(blockVar); // ReferenceError: blockVar is not defined
        console.log(anotherBlockVar); // ReferenceError: anotherBlockVar is not defined
        console.log(notBlockScoped); // "I'm not block scoped" (var ignores block scope)
- Lexical Scope
    - Lexical scope (also called static scope) means that the scope of a variable is determined by its position in the source code.
    ```js
        function outer() {
          const outerVar = "I'm in the outer function";
          function inner() {
            // inner() has access to variables in its own scope, 
            // plus variables in the enclosing scope (outer)
            console.log(outerVar); // Accessible
          }
          inner();
        }
        outer();
- Hoisting
    - Hoisting is JavaScript's default behavior of moving declarations to the top of their containing scope during compilation.
        - variable Hoisting
        ```js
        console.log(hoistedVar); // undefined (not ReferenceError)
        var hoistedVar = "I'm hoisted"; 
        console.log(notHoisted); // ReferenceError: Cannot access before initialization
        let notHoisted = "I'm not hoisted";
        ```
        - Function Declaration Hoisting
        ```js
            // This works because function declarations are hoisted completely
            sayHello(); // "Hello!"

            function sayHello() {
              console.log("Hello!");
            }

            // Function expressions are not hoisted the same way
            sayHi(); // TypeError: sayHi is not a function

            var sayHi = function() {
              console.log("Hi!");
            };
        ```
- Key Differences in Hoisting
    - var variables:
        - Declaration is hoisted (initialized with undefined)
        - No block scope (function scope only)
        - Can be redeclared without error
    - let and const variables:
        - Declaration is hoisted but not initialized (temporal dead zone)
        - Block scoped
        - Cannot be redeclared in the same scope
    - Function declarations:
        - Fully hoisted with their implementation
        - Can be called before declaration
    - Function expressions:
        - Not hoisted (or only the variable declaration is hoisted if using var)
        - Cannot be called before declaration
### TemporalDeadZone
- period between entering a scope where a variable is declared and the actual declaration is called the Temporal Dead Zone.
```js
{
  // TDZ starts for x
  console.log(x); // ReferenceError: Cannot access 'x' before initialization
  let x = 5; // TDZ ends for x
  console.log(x); // 5
}
```

### ScopeChain
- When JavaScript tries to find a variable, it looks up through the nested scopes until it finds it or reaches the global scope.
```js
const global = "I'm global";

function outer() {
  const outerVar = "I'm in outer";
  function inner() {
    const innerVar = "I'm in inner";
    // JavaScript looks for variables in this order:
    // 1. inner scope
    // 2. outer scope
    // 3. global scope
    console.log(innerVar); // Found in inner scope
    console.log(outerVar); // Found in outer scope
    console.log(global);   // Found in global scope
  }
  inner();
}
outer();
```
### Examples
- Var in loops
```js
// This shows var's lack of block scope
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i); // 3, 3, 3 (all reference the same i)
  }, 100);
}

// With let, each iteration gets its own scope
for (let j = 0; j < 3; j++) {
  setTimeout(() => {
    console.log(j); // 0, 1, 2 (each has its own j)
  }, 100);
}
```

- Function Hoisting
```js
// With function declarations
function getFullName(firstName, lastName) {
  return `${firstName} ${lastName}`;
}

// With function expressions
const getFullNameExpression = function(firstName, lastName) {
  return `${firstName} ${lastName}`;
}

// With arrow functions
const getFullNameArrow = (firstName, lastName) => {
  return `${firstName} ${lastName}`;
}

// Only the first function can be called before its definition
console.log(hoistedFunction()); // "I'm hoisted"
// console.log(notHoistedFunction()); // ReferenceError

function hoistedFunction() {
  return "I'm hoisted";
}

const notHoistedFunction = function() {
  return "I'm not hoisted";
};
```

<hr><hr>

### Difference between global and local scope?
- Global scope:
    - Variables and functions declared outside any function or block
    - Accessible from anywhere in the code, including inside functions
    - Exist for the entire lifetime of the application
    - Declared at the top level of a script or with implicit global declaration (without var, let, or const)
- Local scope:
    - Variables and functions declared inside a function
    - Only accessible within that function and any nested functions
    - Created when the function is called and destroyed when the function completes
```js
    // Global scope
    const globalVar = "I'm global";
    function myFunction() {
      // Local scope
      const localVar = "I'm local";
    
      console.log(globalVar);  // Can access global variables
      console.log(localVar);   // Can access local variables
    }
    myFunction();
    console.log(globalVar);    // Can access global variables
    console.log(localVar);     // ReferenceError: localVar is not defined
```
Scope separation helps prevent naming conflicts and unintended variable modifications. Local scope provides data encapsulation and helps keep the global namespace clean.

### What is block scope?
- Block scope refers to the visibility of variables declared within a block (code enclosed in curly braces {}), such as if statements, loops, or standalone blocks.
- Block scope was introduced in ES6 with let and const to provide finer-grained control over variable visibility and lifetime, reducing the risk of variable leakage and unintended side effects.
```js
    {
      // This is a block
      let blockScoped = "I'm block scoped";
      const alsoBlockScoped = "I'm also block scoped";
      var notBlockScoped = "I'm not block scoped";
    
      console.log(blockScoped);      // Accessible
      console.log(alsoBlockScoped);  // Accessible
    }

    console.log(blockScoped);      // ReferenceError: blockScoped is not defined
    console.log(alsoBlockScoped);  // ReferenceError: alsoBlockScoped is not defined
    console.log(notBlockScoped);   // "I'm not block scoped" (var ignores block scope)
```
### Difference between let scope and var scope?
- var scope:
    - Function-scoped (or global if outside a function)
    - Ignores block boundaries (if, for, while blocks)
    - Hoisted to the top of its function and initialized with undefined
    - Can be redeclared in the same scope without errors
    - Accessible throughout its containing function
- let scope:
    - Block-scoped (limited to the block it's defined in)
    - Respects block boundaries
    - Hoisted but not initialized (exists in temporal dead zone until declaration)
    - Cannot be redeclared in the same scope
    - Only accessible after declaration within its block
```js
    function scopeExample() {
      // var example
      console.log(varVar); // undefined (hoisted and initialized)
      if (true) {
        var varVar = "var variable";
      }
      console.log(varVar); // "var variable" (accessible outside the if block)
    
      // let example
      console.log(letVar); // ReferenceError: Cannot access before initialization
      if (true) {
        let letVar = "let variable";
      }
      console.log(letVar); // ReferenceError: letVar is not defined (block-scoped)
    }
```
- var was the original variable declaration in JavaScript, while let was introduced in ES6 to address issues with var, particularly its lack of block scope which could lead to unexpected behaviors and bugs.

### What is lexical scope?
- Lexical scope (also called static scope) means that the scope of variables is determined by their position in the source code (where variables and blocks of scope are authored).
```js
    const globalVar = "I'm global";
    function outerFunction() {
      const outerVar = "I'm in outer function";
    
      function innerFunction() {
        const innerVar = "I'm in inner function";

        console.log(globalVar);  // Access global variable
        console.log(outerVar);   // Access parent function variable
        console.log(innerVar);   // Access own variable
      }
    
      innerFunction();
    }
    outerFunction();
```
- Lexical scoping enables closures and provides predictable variable access patterns since scope is determined by the physical structure of the code rather than the runtime call stack. This allows JavaScript to resolve variable references efficiently during compilation.

### What is hoisting in JavaScript?
- Hoisting is JavaScript's behavior of moving declarations to the top of their containing scope during the compilation phase, before code execution.
```js
// What you write:
console.log(hoistedVar); // undefined
var hoistedVar = "I'm hoisted";

// What JavaScript sees:
var hoistedVar;
console.log(hoistedVar); // undefined
hoistedVar = "I'm hoisted";
```
- Hoisting is a result of how JavaScript's compilation process works. It helps the JavaScript engine allocate memory for variables and functions before executing code, enabling function declarations to be used before they appear in the source code.

### Are function declarations hoisted?
- Yes, function declarations are fully hoisted, meaning both their declaration and implementation are moved to the top of their containing scope.
```js
// This works because of hoisting
sayHello(); // "Hello!"
function sayHello() {
  console.log("Hello!");
}

// Equivalent to:
function sayHello() {
  console.log("Hello!");
}
sayHello(); // "Hello!"
```
- Full hoisting of function declarations enables a coding style where implementation details can follow higher-level function calls, improving code readability in some cases by putting the "what" before the "how."

### Are arrow functions hoisted?
- Arrow functions are not hoisted in the same way as function declarations. If they're assigned to a variable, they follow the hoisting rules of that variable (var, let, or const).
```js
// This won't work
sayHello(); // TypeError: sayHello is not a function

var sayHello = () => {
  console.log("Hello!");
};

// With let or const, you get a different error
sayHi(); // ReferenceError: Cannot access 'sayHi' before initialization

const sayHi = () => {
  console.log("Hi!");
};
```
- Arrow functions are expressions, not declarations. Only the variable declaration is hoisted (if using var), but the function assignment happens at runtime during code execution.

### What is the temporal dead zone?
- The temporal dead zone (TDZ) is the period between entering a scope where a variable is declared with let or const and the actual declaration statement. During this time, the variable exists but cannot be accessed.
- The TDZ helps catch programming errors by throwing a clear error when variables are accessed before declaration, rather than returning the confusing undefined value that var provides.
```js
{
  // TDZ for myVar starts here
  console.log(myVar); // ReferenceError: Cannot access 'myVar' before initialization
  
  let myVar = "TDZ example"; // TDZ ends here
  console.log(myVar); // "TDZ example" (accessible after declaration)
}
```
### How does var behave inside loops?
- Variables declared with var inside loops don't have block scope, which can lead to unexpected behavior, especially with closures.
- With var, there's only one i variable shared across all iterations, and its final value is 3. With let, each loop iteration creates a new binding, capturing the current value for each closure.
```js
// Problem with var in loops:
function createFunctions() {
  var functions = [];
  
  for (var i = 0; i < 3; i++) {
    functions.push(function() {
      console.log(i);
    });
  }
  
  return functions;
}

var funcs = createFunctions();
funcs[0](); // 3
funcs[1](); // 3
funcs[2](); // 3

// Solution with let:
function createFunctionsFixed() {
  var functions = [];
  
  for (let j = 0; j < 3; j++) {
    functions.push(function() {
      console.log(j);
    });
  }
  
  return functions;
}

var fixedFuncs = createFunctionsFixed();
fixedFuncs[0](); // 0
fixedFuncs[1](); // 1
fixedFuncs[2](); // 2
```
### What is scope chain in JS?
- The scope chain is the hierarchical structure of nested scopes in JavaScript that determines variable access. When referencing a variable, JavaScript looks up through the chain until it finds the variable or reaches the global scope.
- scope chain enables lexical scoping and closures by maintaining access to variables from parent scopes, allowing for powerful programming patterns while preventing variable name collisions.
```js
const global = "I'm global";

function outer() {
  const outerVar = "I'm in outer";
  
  function inner() {
    const innerVar = "I'm in inner";
    
    function deepest() {
      const deepestVar = "I'm in deepest";
      
      // JavaScript searches up the scope chain:
      // 1. deepest scope
      // 2. inner scope
      // 3. outer scope
      // 4. global scope
      console.log(innerVar);    // Found in inner scope
      console.log(outerVar);    // Found in outer scope
      console.log(global);      // Found in global scope
      console.log(unknownVar);  // ReferenceError: unknownVar is not defined
    } 
    deepest();
  }
  inner();
}
outer();
```




