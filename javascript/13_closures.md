## Closures in Javascript
- feature which allows functions to retain access of the variables from their outer (enclosing) scope even after the outer function has been returned.

What is a Closure?
A closure is the combination of a function and the lexical environment within which that function was declared. This environment consists of any local variables that were in-scope at the time the closure was created.
```js
function outerFunction() {
  const outerVariable = "I'm from the outer scope";
  
  function innerFunction() {
    console.log(outerVariable); // Access variable from outer scope
  }
  
  return innerFunction;
}

const closure = outerFunction();
closure(); // Logs: "I'm from the outer scope"
```
How Closures Work
- When a function is created in JavaScript, it forms a closure by capturing references to all variables in its lexical scope. Even when the outer function has finished executing, the inner function maintains access to those variables.
- Key points:
    - The inner function has access to its own scope, the outer function's scope, and the global scope
    - The variables in the outer function are preserved as long as the closure exists
    - Each closure gets its own unique scope instance

### Examples
1. DataPrivacy / PrivateVariables
```js
    function createCounter() {
      let count = 0; // Private variable
    
      return {
        increment: function() {
          count++;
          return count;
        },
        decrement: function() {
          count--;
          return count;
        },
        getValue: function() {
          return count;
        }
      };
    }

    const counter = createCounter();
    console.log(counter.increment()); // 1
    console.log(counter.increment()); // 2
    console.log(counter.decrement()); // 1
    console.log(counter.getValue());  // 1

    // count is not directly accessible
    console.log(counter.count);      // undefined
```

2. Event Handlers
```js
    function setupButton(buttonId, message) {
      const button = document.getElementById(buttonId);
    
      button.addEventListener('click', function() {
        // Closure captures the message variable
        console.log(message);
      });
    }

    setupButton('btn1', 'Hello from Button 1');
    setupButton('btn2', 'Greetings from Button 2');
```

3. callbacks with preserved context
```js
    function fetchData(url, processData) {
      const apiKey = 'secret-key-123'; // This will be enclosed in the closure
    
      fetch(url)
        .then(response => response.json())
        .then(data => {
          // Closure captures apiKey from outer scope
          processData(data, apiKey);
        });
    }

    fetchData('https://api.example.com/data', (data, apiKey) => {
      console.log('Processing with key:', apiKey);
      // Process data here
    });
```
### Memory Considerations
- closures can lead to memory issues if not used carefully.
```js
    function createLargeClosures() {
      // Large data structure
      const largeData = new Array(1000000).fill('X');
    
      return function() {
        // This inner function creates a closure over largeData
        console.log(largeData.length);
      };
    }

    // This keeps largeData in memory even if we only need the length
    const getLargeDataLength = createLargeClosures();
    getLargeDataLength(); // 1000000
```
// Better Approach
```js
    function betterApproach() {
      // Large data structure
      const largeData = new Array(1000000).fill('X');
      const length = largeData.length;
    
      // Only capture what we need
      return function() {
        console.log(length);
      };
    }
```

### Common closure Patterns
- Module Pattern
```js
    const calculator = (function() {
      // Private variables
      let result = 0;
    
      // Public interface
      return {
        add: function(x) {
          result += x;
          return this;
        },
        subtract: function(x) {
          result -= x;
          return this;
        },
        multiply: function(x) {
          result *= x;
          return this;
        },
        getResult: function() {
          return result;
        }
      };
    })();

    calculator.add(5).multiply(2).subtract(3);
    console.log(calculator.getResult()); // 7
```

currying with closure
```js
function multiply(a) {
  return function(b) {
    return a * b;
  };
}

const multiplyByTwo = multiply(2);
const multiplyByThree = multiply(3);

console.log(multiplyByTwo(4));   // 8
console.log(multiplyByThree(4)); // 12
```

### Loop Variables in Closures
```js
// Problem:
    function createFunctions() {
      var funcs = [];
    
      for (var i = 0; i < 3; i++) {
        funcs.push(function() {
          console.log(i);
        });
      }
    
      return funcs;
    }

    const functions = createFunctions();
    functions[0](); // 3
    functions[1](); // 3
    functions[2](); // 3

// Solution using let (block scope):
    function createFunctionsFixed() {
      const funcs = [];
    
      for (let i = 0; i < 3; i++) {
        funcs.push(function() {
          console.log(i);
        });
      }
    
      return funcs;
    }   

    const fixedFunctions = createFunctionsFixed();
    fixedFunctions[0](); // 0
    fixedFunctions[1](); // 1
    fixedFunctions[2](); // 2
```
### This context in closures
```js
const user = {
  name: "John",
  sayHi: function() {
    setTimeout(function() {
      // 'this' here refers to the global object, not 'user'
      console.log(`Hi, my name is ${this.name}`);
    }, 1000);
  },
  
  sayHiFixed: function() {
    // Solution 1: Use arrow function
    setTimeout(() => {
      console.log(`Hi, my name is ${this.name}`);
    }, 1000);
    
    // Solution 2: Use bind
    setTimeout(function() {
      console.log(`Hi, my name is ${this.name}`);
    }.bind(this), 1000);
    
    // Solution 3: Store this
    const self = this;
    setTimeout(function() {
      console.log(`Hi, my name is ${self.name}`);
    }, 1000);
  }
};
```
- NOTE:_Closures are a fundamental concept in JavaScript that enable powerful programming patterns like data encapsulation, function factories, and module patterns.

<hr><hr>

### What is a closure in JavaScript?
- closure is a function that retains access to variables from its outer (enclosing) scope even after the outer function has finished executing.
```js
function outerFunction() {
  const outerVariable = "I'm from outer scope";
  
  function innerFunction() {
    console.log(outerVariable); // Still accessible!
  }
  
  return innerFunction;
}

const closure = outerFunction();
closure(); // Logs: "I'm from outer scope"
```
- Reason: Closures exist because of JavaScript's lexical scoping rules. When a function is created, it forms a closure by maintaining references to all variables in its lexical environment. This allows inner functions to "remember" the environment in which they were created, even when executed outside that environment.

### How do closures help with data privacy?
- Closures provide a way to create private variables that cannot be accessed directly from outside code.
```js
function createBankAccount(initialBalance) {
  let balance = initialBalance; // Private variable
  
  return {
    deposit: function(amount) {
      balance += amount;
      return `Deposited ${amount}. New balance: ${balance}`;
    },
    withdraw: function(amount) {
      if (amount > balance) {
        return "Insufficient funds";
      }
      balance -= amount;
      return `Withdrew ${amount}. New balance: ${balance}`;
    },
    getBalance: function() {
      return balance;
    }
  };
}

const account = createBankAccount(100);
console.log(account.getBalance()); // 100
console.log(account.deposit(50));  // "Deposited 50. New balance: 150"
console.log(account.balance);      // undefined (can't access directly)
```
Reason: By encapsulating variables within a function scope and only exposing controlled access methods, closures implement data privacy in JavaScript. This pattern forms the basis of the module pattern and is essential for creating secure, maintainable code with proper encapsulation.

### Can closures lead to memory leaks?
- Yes, closures can lead to memory leaks if not used carefully.
```js
function createPotentialLeak() {
  // Large data structure (10MB array)
  const largeData = new Array(1000000).fill('X');
  
  function processData() {
    // Uses the large data from parent scope
    console.log(largeData.length);
  }
  
  return processData;
}

// This retains the entire largeData array in memory
const leak = createPotentialLeak();
leak(); // 1000000
```
- Reason: Since closures maintain references to their entire lexical environment, they prevent variables in that scope from being garbage collected. Common issues include:
    - Capturing large objects when only small portions are needed
    - Creating circular references (especially with DOM elements)
    - Long-lived event handlers that are never properly removed
    - Capturing variables unintentionally in large scopes

### How do you use closures in event handlers?
Closures in event handlers allow you to preserve variables and context from when the handler was created:
```js
function setupButtons() {
  // Using closures to maintain separate data for each button
  for (let id = 1; id <= 3; id++) {
    const button = document.createElement('button');
    button.textContent = `Button ${id}`;
    
    // This event handler forms a closure over the 'id' variable
    button.addEventListener('click', function() {
      console.log(`Button ${id} was clicked`);
      // Can access 'id' even though setupButtons has finished executing
    });
    
    document.body.appendChild(button);
  }
}

// Later, when buttons are clicked:
// "Button 1 was clicked"
// "Button 2 was clicked"
// "Button 3 was clicked"
```
- Reason: Event handlers execute at some point in the future, long after their defining code has completed. Closures ensure they still have access to the correct variables and context from when they were created. This allows for:
    - Preserving specific data for each handler
    - Avoiding global variables for state management
    - Creating dynamic handlers with different behaviors

### Example: counter function using closures?
```js
function createCounter(initialValue = 0) {
  // Private variable that persists between function calls
  let count = initialValue;
  
  return {
    increment: function() {
      count++;
      return count;
    },
    decrement: function() {
      count--;
      return count;
    },
    reset: function() {
      count = initialValue;
      return count;
    },
    getValue: function() {
      return count;
    }
  };
}

// Usage
const counter = createCounter(5);
console.log(counter.getValue());  // 5
console.log(counter.increment()); // 6
console.log(counter.increment()); // 7
console.log(counter.decrement()); // 6
console.log(counter.reset());     // 5

// Creating a second independent counter
const counter2 = createCounter(10);
console.log(counter2.getValue()); // 10
console.log(counter.getValue());  // 5 (still has its own state)
```
- Reason: This counter example demonstrates several key features of closures:
    - Data encapsulation: The count variable is private and cannot be accessed directly
    - State persistence: The count value persists between function calls
    - Multiple instances: Each call to createCounter creates a new closure with its own independent state
    - Controlled access: The inner state can only be modified through the provided methods




