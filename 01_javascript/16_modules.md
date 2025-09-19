## JavaScript Modules
JavaScript modules allow you to break up your code into separate files to make it more maintainable, reusable, and organized. They provide a way to export and import functionality between different parts of an application.

### Introduction to Modules
- Modules help solve several problems in JavaScript development:
    - Organizing code into separate files
    - Avoiding global namespace pollution
    - Creating reusable components
    - Managing dependencies
    - Encapsulating implementation details
### Module Systems in JavaScript
- CommonJS (Node.js)
    ```js
    // Exporting in CommonJS
        const PI = 3.14159;
        function circleArea(radius) {
          return PI * radius * radius;
        }

        module.exports = {
          PI,
          circleArea
        };

        // Importing in CommonJS
        const math = require('./math');
        console.log(math.circleArea(5));
    ```
- ES Modules (Modern JavaScript)
    ```js
    // Exporting in ES Modules
        export const PI = 3.14159;
        export function circleArea(radius) {
          return PI * radius * radius;
        }

        // Importing in ES Modules
        import { PI, circleArea } from './math.js';
        console.log(circleArea(5));
    ```
### ES Modules in Detail
- Named Exports
    ```js
        // Multiple named exports
        export const name = 'Calculator';
        export function add(a, b) { return a + b; }
        export function subtract(a, b) { return a - b; }

        // Alternative syntax
        const name = 'Calculator';
        function add(a, b) { return a + b; }
        function subtract(a, b) { return a - b; }

        export { name, add, subtract };
    ```
- Default Exports
    ```js
        // Default export
        export default class User {
          constructor(name, email) {
            this.name = name;
            this.email = email;
          }
        }

        // Importing default export
        import User from './User.js';
        const user = new User('John', 'john@example.com');
    ```
- Combining Named and Default Exports
    ```js
        // Exporting both default and named exports
        export const VERSION = '1.0.0';
        export function helper() { /* ... */ }

        export default class API {
          // API implementation
        }

        // Importing both
        import API, { VERSION, helper } from './api.js';
    ```
- Namespace Imports
    ```js
        // Import everything as a namespace object
        import * as mathUtils from './math.js';

        console.log(mathUtils.PI);
        console.log(mathUtils.circleArea(5));
    ```
- Renaming Imports and Exports
    ```js
    // Renaming exports
        export { add as sum, subtract as difference };

        // Renaming imports
        import { add as sum, subtract as difference } from './math.js';
    ```
- Re-exporting
    ```js
    // Re-exporting from another module
        export { default as User } from './User.js';
        export { format, parse } from './date-utils.js';
    ```

### Dynamic Imports
```js
// Dynamic import (returns a promise)
async function loadModule() {
  try {
    const math = await import('./math.js');
    console.log(math.circleArea(5));
  } catch (error) {
    console.error('Error loading module:', error);
  }
}

loadModule();
```
### Module Scope
```js
// Variables defined in a module are scoped to that module
const privateVar = 'This is not accessible outside';
let counter = 0;

export function incrementCounter() {
  counter++;
  return counter;
}

export function getCounter() {
  return counter;
}
```
### Using ES Modules in Browsers
```js
<!-- Using modules in HTML -->
<script type="module">
  import { greeting } from './greeting.js';
  console.log(greeting('World'));
</script>

<!-- Importing a module from a URL -->
<script type="module">
  import { Component } from 'https://unpkg.com/library/module.js';
  const component = new Component();
</script>
```
### Module Bundlers
- Popular module bundlers for production applications:
    - Webpack
    - Rollup
    - Parcel
    - esbuild
    - Vite
```js
// Example webpack.config.js
module.exports = {
  entry: './src/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [
      {
        test: /\.js$/,
        exclude: /node_modules/,
        use: {
          loader: 'babel-loader'
        }
      }
    ]
  }
};
```
### Pre-ES6 Module Patterns
- Immediately Invoked Function Expression (IIFE)
```js
// IIFE Module Pattern
const calculator = (function() {
  // Private variables
  const privateValue = 42;
  
  // Public API
  return {
    add: function(a, b) {
      return a + b;
    },
    subtract: function(a, b) {
      return a - b;
    },
    getPrivateValue: function() {
      return privateValue;
    }
  };
})();

console.log(calculator.add(5, 3)); // 8
console.log(calculator.getPrivateValue()); // 42
```
- AMD (Asynchronous Module Definition)
```js
// AMD module format
define(['dependency1', 'dependency2'], function(dep1, dep2) {
  return {
    method1: function() {
      return dep1.doSomething();
    },
    method2: function() {
      return dep2.doSomethingElse();
    }
  };
});
```
- UMD (Universal Module Definition)
```js
// UMD module pattern (works in both Node.js and browser)
(function(root, factory) {
  if (typeof define === 'function' && define.amd) {
    // AMD
    define(['dependency'], factory);
  } else if (typeof module === 'object' && module.exports) {
    // CommonJS
    module.exports = factory(require('dependency'));
  } else {
    // Browser globals
    root.myModule = factory(root.dependency);
  }
}(typeof self !== 'undefined' ? self : this, function(dependency) {
  // Module code goes here
  return {
    // Public API
  };
}));
```
### Best Practices for JavaScript Modules
- Use ES modules when possible for modern applications
- Keep modules focused on a single responsibility
- Use explicit imports instead of namespace imports for better tree-shaking
- Avoid side effects in modules
- Consider using named exports for better refactoring and IDE support
- Use consistent naming between files and their default exports
- Use module bundlers for production applications
- Consider code splitting for larger applications

### Module Resolution
```js
// Relative path imports
import { helper } from './utils.js';
import { Component } from '../components/Component.js';

// Absolute path imports (with proper configuration)
import { api } from 'services/api.js';
import { Button } from 'components/Button.js';

// Package imports
import React from 'react';
import { useState } from 'react';
```
### TypeScript and Modules
```js
// TypeScript with ES modules
export interface User {
  id: number;
  name: string;
  email: string;
}

export function createUser(name: string, email: string): User {
  return {
    id: Date.now(),
    name,
    email
  };
}
```

</hr></hr>

### Difference between CommonJS and ES6 modules?
- CommonJs
```js
// Exporting in CommonJS
const PI = 3.14159;
function circleArea(radius) {
  return PI * radius * radius;
}

module.exports = { PI, circleArea };

// Importing in CommonJS
const math = require('./math');
console.log(math.circleArea(5));
```
- ES_6 Modules
```js
// Exporting in ES Modules
export const PI = 3.14159;
export function circleArea(radius) {
  return PI * radius * radius;
}

// Importing in ES Modules
import { PI, circleArea } from './math.js';
console.log(circleArea(5));
```
- Key differences and reasons:
    - Syntax: CommonJS uses require() and module.exports, while ES6 uses import and export keywords
    - Loading: CommonJS loads modules synchronously at runtime, while ES6 modules are parsed before execution (static)
    - Environment: CommonJS was designed for server (Node.js), while ES6 modules work in both browsers and Node.js
    - Static Analysis: ES6 imports/exports are determined at compile time, enabling optimizations like tree-shaking
    - Hoisting: ES6 imports are hoisted, CommonJS requires are not
    - Default Exports: ES6 has a cleaner syntax for default exports
    - Top-level await: ES6 modules support top-level await, CommonJS doesn't

### What does export default do?
```js
// Exporting a default value
export default class User {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }
}

// Importing a default export
import User from './User.js';
const user = new User('John', 'john@example.com');
```
Reasons:
    - export default allows you to export a single main value, function, or class from a module
    - A module can only have one default export
    - It simplifies imports as you don't need curly braces to import the default export
    - Default exports are useful when a module primarily provides one thing
    - You can name the import whatever you want (unlike named imports which must match)

### How do you import specific functions from a module?
```js
// Named exports in a module
export const name = 'Calculator';
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }

// Importing specific functions
import { add, subtract } from './math.js';
console.log(add(5, 3)); // 8

// Renaming imports
import { add as sum, subtract as difference } from './math.js';
console.log(sum(5, 3)); // 8
```
- Reasons:
    - Use curly braces {} to select specific named exports
    - This enables more precise imports and better tree-shaking
    - You can rename imports using the as keyword to avoid naming conflicts
    - Only the functions you specifically import are included in your bundle
### What is tree-shaking?
- Tree-shaking is an optimization technique that removes unused code from your final bundle.
```js
// utils.js
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }
export function multiply(a, b) { return a * b; }

// main.js
import { add } from './utils.js';
console.log(add(5, 3));
```
- In this example, a bundler with tree-shaking would only include the add function in the final bundle, omitting subtract and multiply.
- Reasons:
    - Tree-shaking works because ES6 modules are statically analyzable
    - It significantly reduces bundle sizes by eliminating "dead code"
    - Only possible with ES Modules (not with CommonJS)
    - Modern bundlers like Webpack, Rollup, and esbuild support it
    - Helps improve application performance and loading times

### Can you use dynamic imports?
- Yes, dynamic imports are supported in modern JavaScript using the import() function:
```js
// Dynamic import (returns a promise)
async function loadModule() {
  try {
    const math = await import('./math.js');
    console.log(math.circleArea(5));
  } catch (error) {
    console.error('Error loading module:', error);
  }
}

// Conditional imports
if (userPreference === 'advanced') {
  import('./advanced-features.js')
    .then(module => {
      module.initAdvancedFeatures();
    });
}
```
- Reasons:
    - Dynamic imports return a Promise, so you can use async/await or .then()
    - They enable code-splitting and lazy-loading of modules
    - Modules are loaded on-demand rather than upfront
    - Useful for conditional loading based on user actions or preferences
    - Improves initial load performance by only loading what's needed
    - Supported in modern browsers and Node.js

