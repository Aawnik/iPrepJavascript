## Error Handling
- Error Handling, allows us to gracefully manage unexpected situations rather than letting your application crash.

### Types of errors
- Built-In Error
```js
  // Error - Base error type
  const genericError = new Error("Something went wrong");

  // TypeError - When a value is not of the expected type
  const typeError = new TypeError("Expected a string, got a number");

  // SyntaxError - When code is not valid JavaScript
  // Cannot be caught with try-catch as it occurs during parsing
  // const syntaxError = new SyntaxError("Missing closing bracket");

  // ReferenceError - When referencing an undeclared variable
  const referenceError = new ReferenceError("x is not defined");

  // RangeError - When a value is outside the allowed range
  const rangeError = new RangeError("Invalid array length");

  // URIError - When encodeURI() or decodeURI() are used incorrectly
  const uriError = new URIError("Invalid URI");
```
- Try-Catch-Finally
```js
  try {
    // Code that might throw an error
    const data = JSON.parse('{"name": "John"}');
    console.log(data.name); // Works fine
  } catch (error) {
    // Handle the error
    console.error("An error occurred:", error.message);
  } finally {
    // Always executes, regardless of error
    console.log("Cleanup operations");
  }
```
Throwing Custom Errors
```js
  function divide(a, b) {
    if (b === 0) {
      throw new Error("Division by zero is not allowed");
    }
    return a / b;
  }
  try {
    console.log(divide(10, 0));
  } catch (error) {
    console.error(error.message); // "Division by zero is not allowed"
  }
```
- Creating Custom Error
```js
  class ValidationError extends Error {
    constructor(message) {
      super(message);
      this.name = "ValidationError";
      // Maintains proper stack trace in V8 engines
      if (Error.captureStackTrace) {
        Error.captureStackTrace(this, ValidationError);
      }
    }
  }

  try {
    throw new ValidationError("Form validation failed");
  } catch (error) {
    if (error instanceof ValidationError) {
      console.log("Handle validation error:", error.message);
    } else {
      console.log("Handle other error");
    }
  }
```
### Advanced Error Handling
- Handling Async Errors
- For Callbacks
```js
  function fetchData(callback) {
    setTimeout(() => {
      try {
        // Simulating an error
        throw new Error("Network failure");
      } catch (error) {
        callback(error, null);
      }
    }, 1000);
  }

  fetchData((error, data) => {
    if (error) {
      console.error("Error:", error.message);
    } else {
      console.log("Data:", data);
    }
  });
```
- For Promises
```js
  function fetchUserData() {
    return new Promise((resolve, reject) => {
      // Simulating an API call
      setTimeout(() => {
        const success = false;
        if (success) {
          resolve({ id: 1, name: "John" });
        } else {
          reject(new Error("Failed to fetch user"));
        }
      }, 1000);
    });
  }

  fetchUserData()
    .then(user => console.log("User:", user))
    .catch(error => console.error("Error:", error.message));
```
- USing Async/Await
```js
  async function getUserData() {
    try {
      const response = await fetch('https://api.example.com/user');
      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }
      const user = await response.json();
      return user;
    } catch (error) {
      console.error("Failed to get user data:", error.message);
      // Re-throw or return a default value
      throw error; // Or: return { name: "Guest" };
    }
  }
```
- Global Error Handling
```js
  // Unhandled promise rejections
  window.addEventListener('unhandledrejection', function(event) {
    console.error('Unhandled promise rejection:', event.reason);
    // Prevent default handling
    event.preventDefault();
  });

  // Global error handler
  window.onerror = function(message, source, lineno, colno, error) {
    console.error(`Error: ${message} at ${source}:${lineno}:${colno}`);
    // Return true to prevent the default browser error handling
    return true;
  };
```
### Debugging tool & Techniques
```js
  // Using console methods
  console.error("Critical error occurred");
  console.warn("Warning: deprecated feature used");
  console.trace("Show call stack");

  // Conditional debugging
  console.assert(1 === 2, "This will show an error message");

  // Debugger statement
  function buggyFunction() {
    let x = 5;
    debugger; // Browser will pause execution here when dev tools are open
    x = x + 10;
    return x;
  }
```
### Error HAndling Best Practice
- Be specific about what you catch: Catch only errors you can handle
- Don't swallow errors: Always log or handle errors meaningfully
- Clean up resources in finally blocks: Ensure proper cleanup regardless of errors
- Use custom error types: For better error categorization and handling
- Add meaningful error messages: Help developers understand what went wrong

### Example : Form Vallidation with ErrorHandling
```js
  document.getElementById('userForm').addEventListener('submit', async function(event) {
    event.preventDefault();
    const username = document.getElementById('username').value;
    const email = document.getElementById('email').value;
    const errorDisplay = document.getElementById('errorDisplay');

    try {
      // Clear previous errors
      errorDisplay.textContent = '';

      // Validate input
      if (!username) {
        throw new ValidationError("Username is required");
      }

      if (!email || !email.includes('@')) {
        throw new ValidationError("Valid email is required");
      }

      // Simulate API call
      const result = await submitToServer(username, email);
      showSuccess("User registered successfully!");

    } catch (error) {
      if (error instanceof ValidationError) {
        // Handle validation errors (client-side)
        errorDisplay.textContent = error.message;
        errorDisplay.style.color = 'red';
      } else if (error.name === 'NetworkError') {
        // Handle network errors
        errorDisplay.textContent = "Network error. Please try again.";
      } else {
        // Handle unexpected errors
        errorDisplay.textContent = "An unexpected error occurred.";
        console.error(error);
      }
    }
  });

  function submitToServer(username, email) {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        // Simulate success/failure
        if (Math.random() > 0.5) {
          resolve({ success: true, userId: 123 });
        } else {
          const error = new Error("Server error");
          error.name = "NetworkError";
          reject(error);
        }
      }, 1000);
    });
  }

  function showSuccess(message) {
    const successDisplay = document.getElementById('successDisplay');
    successDisplay.textContent = message;
    successDisplay.style.color = 'green';
  }
```

<hr><hr>

### What is a try...catch block?
- A try...catch block is a JavaScript error handling mechanism that allows you to execute code that might throw errors and catch those errors without terminating script execution.
```js
  try {
    // Code that might throw an error
    const data = JSON.parse('{"name": "John"}');
  } catch (error) {
    // Code to handle errors
    console.error("An error occurred:", error.message);
  }
```
- REASON:_Reason: Try...catch blocks prevent application crashes by allowing developers to gracefully handle errors, provide fallback behaviors, and present user-friendly error messages instead of exposing technical details.

### Difference between runtime errors and syntax errors?
- Runtime errors:
  - Occur during program execution
  - Can be caught with try...catch blocks
  - Examples: TypeError, ReferenceError, RangeError
  - Appear after JavaScript has already parsed the code
- Syntax errors:
  - Occur during parsing/compilation before execution
  - Cannot be caught with try...catch blocks
  - Examples: missing brackets, invalid variable names, unexpected tokens
  - Prevent the code from running at all
- Reason: This distinction is important because try...catch can only help with errors that occur during execution, not with code that's syntactically invalid to begin with.

### What is the finally block used for?
- The `finally` block contains code that will execute regardless of whether an exception was thrown or caught in the try...catch block.
- The `finally` block ensures that cleanup operations (like closing connections, releasing resources, resetting UI states) happens even if errors occur, preventing resource leaks and maintaining consistent application state.
```js
  try {
    // Try to do something
    fetch('https://api.example.com/data');
  } catch (error) {
    // Handle any errors
    console.error("Failed to fetch data:", error);
  } finally {
    // This code always runs
    hideLoadingSpinner();
    enableButton();
  }
```

### Can you have multiple catch blocks?
- In modern JavaScript, you cannot have multiple catch blocks directly. However, you can achieve similar functionality by using a single catch block with conditional logic:
```js
  try {
    // Code that might throw different types of errors
  } catch (error) {
    if (error instanceof TypeError) {
      // Handle TypeError
    } else if (error instanceof RangeError) {
      // Handle RangeError
    } else {
      // Handle other errors
    }
  }
```
- Note: In older JavaScript implementations (prior to ES2019), there was a try-catch-finally syntax that allowed catching different error types, but this has been removed from the language.
- Reason: While multiple catch blocks would be convenient, JavaScript's approach encourages explicit error type checking, which can lead to more intentional error handling.

### How do you throw custom errors?
- You can throw custom errors using the throw statement with either:
  - The built-in Error constructor
  - Custom error classes that extend Error
- Custom errors allow you to distinguish between different types of errors in your application, making error handling more specific and informative for debugging.
```js
  // Using the built-in Error constructor
  function divide(a, b) {
    if (b === 0) {
      throw new Error("Division by zero is not allowed");
    }
    return a / b;
  }

  // Creating custom error classes
  class ValidationError extends Error {
    constructor(message) {
      super(message);
      this.name = "ValidationError";
      // Maintains proper stack trace in V8 engines
      if (Error.captureStackTrace) {
        Error.captureStackTrace(this, ValidationError);
      }
    }
  }

  function validateUser(user) {
    if (!user.name) {
      throw new ValidationError("Name is required");
    }
  }
```
### What is the difference between Error and TypeError?
- Error:
  - Base class for all JavaScript errors
  - General-purpose error object
  - Used for creating custom error types
  - Contains message, name, and stack properties
- TypeError:
  - Subclass of Error
  - Specifically thrown when an operation is performed on a value of the wrong type
  - Indicates type-related issues (e.g., calling a non-function, accessing properties of null/undefined)
- TypeErrors help pinpoint specific issues with data types in code, making debugging easier by immediately identifying when values aren't of the expected type.
```js
  // Error example
  throw new Error("Generic error message");

  // TypeError examples
  null.toString();   // TypeError: Cannot read property 'toString' of null
  123();             // TypeError: 123 is not a function
```
### How do you debug in JavaScript?
There are multiple ways to debug JavaScript:
- Browser Developer Tools:
  - Use breakpoints in Sources/Debugger panel
  - Step through code execution with Step Over, Step Into, Step Out controls
  - Inspect variables in the Scope panel
  - Use the Watch panel to track specific values
- Debugging statements:
  - debugger; statement to pause execution (when dev tools are open)
  - console.log(), console.table(), console.dir() for variable inspection
- Node.js debugging:
  - Use the --inspect flag with Chrome DevTools
  - Use VS Code's built-in JavaScript debugger
- Effective debugging is crucial for identifying and fixing issues in complex applications, and JavaScript provides multiple approaches to suit different scenarios and developer preferences.
```js
  function calculateTotal(items) {
    let total = 0;
    debugger; // Execution pauses here when dev tools are open
    for (const item of items) {
      console.log(`Adding ${item.price}`); // Inspect values at runtime
      total += item.price;
    }
    return total;
  }
```
### What is console.error used for?
- console.error() is a method that outputs an error message to the browser's console or the terminal in Node.js:
```js
try {
  throw new Error("Something went wrong");
} catch (error) {
  console.error("Error occurred:", error.message);
}
```
Features of console.error:
  - Displays text in red (in most browser consoles)
  - Includes stack trace for Error objects
  - Can be filtered separately from regular console logs
  - Shows up in the "Errors" tab in browser developer tools
Reason: `console.error()` provides visual distinction for error messages, making them more noticeable during development and easier to filter when reviewing logs. It's specifically designed for error reporting rather than general logging.

### What is window.onerror?
- `window.onerror` is a global error event handler that catches uncaught JavaScript errors.
- `window.onerror` provides a safety net for capturing and handling errors that weren't caught elsewhere in your application. It's particularly useful for error logging, analytics, and preventing default browser error messages in production environments.
```js
  window.onerror = function(message, source, lineno, colno, error) {
    console.error(`Error: ${message} at ${source}:${lineno}:${colno}`);
    // Send error to analytics service
    sendErrorToAnalytics(message, source, lineno, colno, error);
    // Return true to prevent the browser's default error handling
    return true;
  };
```
### How do you handle async errors in promises?
- Asynchronous error handling requires special approaches because errors in promises won't be caught by regular try...catch blocks unless they're within an async function with await. These patterns ensure errors in asynchronous code are properly handled rather than silently failing.
- There are two main approaches for handling errors in asynchronous code:
  - Using .catch() with Promises:
  ```js
    fetch('https://api.example.com/data')
    .then(response => {
      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }
      return response.json();
    })
    .then(data => {
      console.log('Data:', data);
    })
    .catch(error => {
      console.error('Fetch error:', error);
    });
  ```
  - Using try...catch with async/await:
  ```js
    async function fetchData() {
    try {
      const response = await fetch('https://api.example.com/data');
      if (!response.ok) {
        throw new Error(`HTTP error! Status: ${response.status}`);
      }
      const data = await response.json();
      console.log('Data:', data);
    } catch (error) {
      console.error('Fetch error:', error);
    }
  }
  ```
  - Global unhandled rejection handler:
  ```js
    window.addEventListener('unhandledrejection', event => {
    console.error('Unhandled promise rejection:', event.reason);
    event.preventDefault(); // Prevent default handling
    });
  ```






