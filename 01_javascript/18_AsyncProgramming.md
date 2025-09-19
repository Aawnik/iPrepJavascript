## Asynchronous Programming in JavaScript
JavaScript is primarily a single-threaded language, but it can handle operations asynchronously. This allows code to continue executing while waiting for operations like API calls, timers, or file operations to complete.

Understanding Synchronous vs Asynchronous Code
```js
// Synchronous code - executes sequentially
console.log("First");
console.log("Second");
console.log("Third");
// Output: First, Second, Third

// Asynchronous code - doesn't necessarily execute in sequence
console.log("First");
setTimeout(() => {
  console.log("Second"); // This runs later
}, 1000);
console.log("Third");
// Output: First, Third, Second
```
### Callback Functions
Callbacks are functions passed as arguments to other functions, which are then executed when an operation completes.
```js
// Basic callback example
function fetchData(callback) {
  setTimeout(() => {
    const data = { id: 1, name: "User" };
    callback(data);
  }, 1000);
}

fetchData(function(data) {
  console.log("Data received:", data);
});
console.log("Fetching data...");
```
Callback Hell
```js
// Callback hell example
fetchUserData(function(userData) {
  fetchUserPosts(userData.id, function(posts) {
    fetchPostComments(posts[0].id, function(comments) {
      fetchCommentAuthor(comments[0].userId, function(author) {
        console.log("Author details:", author);
        // More nested callbacks...
      });
    });
  });
});
```
Promises
Promises represent the eventual completion (or failure) of an asynchronous operation and its resulting value.
```js
// Creating a promise
const fetchUserData = new Promise((resolve, reject) => {
  // Asynchronous operation
  setTimeout(() => {
    const success = true;
    
    if (success) {
      resolve({ id: 1, name: "John Doe" });
    } else {
      reject("Error fetching user data");
    }
  }, 1000);
});

// Using promises
fetchUserData
  .then(userData => {
    console.log("User data:", userData);
    return fetchUserPosts(userData.id);
  })
  .then(posts => {
    console.log("User posts:", posts);
    return fetchPostComments(posts[0].id);
  })
  .catch(error => {
    console.error("Error:", error);
  })
  .finally(() => {
    console.log("Operation completed");
  });
```
### Promise Methods
```js
// Promise.all - waits for all promises to resolve
Promise.all([fetchUsers(), fetchPosts(), fetchComments()])
  .then(([users, posts, comments]) => {
    console.log("All data fetched:", { users, posts, comments });
  })
  .catch(error => {
    console.error("At least one promise failed:", error);
  });

// Promise.race - resolves/rejects as soon as one promise resolves/rejects
Promise.race([
  fetchData(),
  new Promise((_, reject) => setTimeout(() => reject("Timeout"), 3000))
])
  .then(result => console.log("Result:", result))
  .catch(error => console.error("Error or timeout:", error));

// Promise.allSettled - waits for all promises to settle (resolve or reject)
Promise.allSettled([fetchUsers(), fetchPosts(), fetchInvalidData()])
  .then(results => {
    results.forEach(result => {
      if (result.status === 'fulfilled') {
        console.log('Fulfilled:', result.value);
      } else {
        console.log('Rejected:', result.reason);
      }
    });
  });
```
### Async/Await
Async/await provides a more readable way to work with promises.
```js
// Async function declaration
async function fetchUserData() {
  try {
    const userData = await fetchUser(1); // Waits for promise to resolve
    const posts = await fetchUserPosts(userData.id);
    const comments = await fetchPostComments(posts[0].id);
    
    return { userData, posts, comments };
  } catch (error) {
    console.error("Error fetching data:", error);
    throw error; // Re-throw or handle as needed
  }
}

// Using async function
fetchUserData()
  .then(data => console.log("All data:", data))
  .catch(error => console.error("Error in async function:", error));

// Async/await with Promise.all
async function fetchAllData() {
  try {
    const [users, posts, comments] = await Promise.all([
      fetchUsers(),
      fetchPosts(),
      fetchComments()
    ]);
    
    console.log({ users, posts, comments });
  } catch (error) {
    console.error("Error:", error);
  }
}
```
### The Event Loop
- JavaScript uses an event loop to handle asynchronous operations:
    - Call Stack: Where synchronous code executes
    - Web APIs: Where async operations run (setTimeout, fetch, etc.)
    - Callback Queue: Where callbacks wait to be processed
    - Microtask Queue: Higher priority queue for promises
    - Event Loop: Moves callbacks to the call stack when it's empty
```js
console.log("Start"); // 1

setTimeout(() => {
  console.log("Timeout callback"); // 4
}, 0);

Promise.resolve()
  .then(() => console.log("Promise resolved")); // 3

console.log("End"); // 2

// Output:
// Start
// End
// Promise resolved
// Timeout callback
```
### Practical Examples
- Fetch API
```js
// Using fetch with promises
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    return response.json();
  })
  .then(data => {
    console.log('Data:', data);
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });

// Using fetch with async/await
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) {
      throw new Error('Network response was not ok');
    }
    
    const data = await response.json();
    console.log('Data:', data);
    return data;
  } catch (error) {
    console.error('Fetch error:', error);
    throw error;
  }
}
```
Loading Data with Status Tracking
```js
async function loadUserDataWithStatus() {
  // Update UI to show loading state
  document.getElementById('status').textContent = 'Loading...';
  
  try {
    // Fetch user data
    const userData = await fetchUser();
    document.getElementById('status').textContent = 'Loading posts...';
    
    // Fetch posts with the user ID
    const posts = await fetchPosts(userData.id);
    
    // Update UI with the fetched data
    document.getElementById('user-name').textContent = userData.name;
    document.getElementById('post-count').textContent = posts.length;
    document.getElementById('status').textContent = 'Loaded successfully';
    
  } catch (error) {
    document.getElementById('status').textContent = `Error: ${error.message}`;
    console.error(error);
  }
}
```
#### Async Patterns
- Debouncing
```js
function debounce(func, wait) {
  let timeout;
  
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

// Usage
const debouncedSearch = debounce(function(term) {
  console.log('Searching for:', term);
  // Perform search operation
}, 300);

// Call in response to user input
searchInput.addEventListener('input', e => {
  debouncedSearch(e.target.value);
});
```
- Parallel vs Sequential Execution
```js
// Sequential execution - one after another
async function sequentialFetch() {
  console.time('sequential');
  
  const users = await fetchUsers();
  const posts = await fetchPosts();
  const comments = await fetchComments();
  
  console.timeEnd('sequential');
  return { users, posts, comments };
}

// Parallel execution - all at once
async function parallelFetch() {
  console.time('parallel');
  
  const [users, posts, comments] = await Promise.all([
    fetchUsers(),
    fetchPosts(),
    fetchComments()
  ]);
  
  console.timeEnd('parallel');
  return { users, posts, comments };
}
```
#### Error Handling in Asynchronous Code
```js
// Multiple catch blocks with async/await
async function fetchWithErrorHandling() {
  try {
    const response = await fetch('https://api.example.com/data');
    
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    
    try {
      const data = await response.json();
      return data;
    } catch (parseError) {
      throw new Error(`JSON parsing error: ${parseError.message}`);
    }
  } catch (networkError) {
    console.error('Network or HTTP error:', networkError);
    throw networkError;
  }
}

// Global error handling for promises
window.addEventListener('unhandledrejection', event => {
  console.error('Unhandled promise rejection:', event.reason);
  // Prevent the default handling
  event.preventDefault();
});
```


<hr></hr>


### Difference between synchronous and asynchronous code?
Synchronous code executes line by line, with each operation completing before the next one begins. Asynchronous code allows operations to run in the background while the rest of the code continues executing.
```js
// Synchronous code
console.log("First");
console.log("Second");
console.log("Third");
// Output: First, Second, Third (in order)

// Asynchronous code
console.log("First");
setTimeout(() => {
  console.log("Second"); // Runs later
}, 0);
console.log("Third");
// Output: First, Third, Second
```
Reason: JavaScript is single-threaded by design. Synchronous operations block execution until they complete, which can make applications unresponsive during time-consuming tasks. Asynchronous operations allow JavaScript to handle potentially blocking operations (network requests, file I/O, timers) without freezing the main thread, enabling responsive user interfaces and efficient resource utilization.
### What are callbacks?
Callbacks are functions passed as arguments to other functions, which are then invoked when an operation completes.
```js
function fetchData(callback) {
  setTimeout(() => {
    const data = { id: 1, name: "User" };
    callback(data);
  }, 1000);
}

fetchData(function(data) {
  console.log("Data received:", data);
});
console.log("Fetching data...");
// Output: "Fetching data..." (immediately)
// Output: "Data received: {id: 1, name: 'User'}" (after 1 second)
```
Reason: Callbacks were the original mechanism for handling asynchronous operations in JavaScript. They enable code to continue running while waiting for an operation to complete, then execute specific logic when the operation finishes. However, they can lead to "callback hell" (deeply nested callbacks) when dealing with multiple sequential asynchronous operations, making code hard to read and maintain.
### What are promises?
Promises are objects representing the eventual completion (or failure) of an asynchronous operation and its resulting value.
```js
const fetchData = new Promise((resolve, reject) => {
  setTimeout(() => {
    const success = true;
    if (success) {
      resolve({ id: 1, name: "User" });
    } else {
      reject("Error fetching data");
    }
  }, 1000);
});

fetchData
  .then(data => {
    console.log("Data:", data);
    return processData(data);
  })
  .then(processedData => {
    console.log("Processed:", processedData);
  })
  .catch(error => {
    console.error("Error:", error);
  })
  .finally(() => {
    console.log("Operation complete");
  });
```
Reason: Promises were introduced to solve the callback hell problem, providing a more structured way to handle asynchronous operations. They have three states: pending, fulfilled (resolved), or rejected. Promises enable chaining with .then() methods for sequential operations and centralized error handling with .catch(), making asynchronous code more readable and maintainable.

### Difference between Promise.all and Promise.race?
Promise.all() takes an array of promises and returns a new promise that fulfills with an array of results when all input promises have fulfilled, or rejects if any input promise rejects.

Promise.race() takes an array of promises and returns a new promise that fulfills or rejects as soon as one of the input promises fulfills or rejects.
```js
// Promise.all example
Promise.all([fetchUsers(), fetchPosts(), fetchComments()])
  .then(([users, posts, comments]) => {
    console.log("All data fetched:", { users, posts, comments });
  })
  .catch(error => {
    console.error("At least one promise failed:", error);
  });

// Promise.race example
Promise.race([
  fetchData(),
  new Promise((_, reject) => setTimeout(() => reject("Timeout"), 3000))
])
  .then(result => console.log("Result:", result))
  .catch(error => console.error("Error or timeout:", error));
```
Reason:
Promise.all is used when multiple asynchronous operations need to complete before proceeding, and you need all their results together.
Promise.race is used when you want to proceed with the first available result, or implement features like timeouts or fallbacks.
### What is async/await?
Async/await is syntactic sugar over promises that makes asynchronous code look and behave more like synchronous code.
```js
// Async function declaration
async function fetchUserData() {
  try {
    const userData = await fetchUser(1); // Waits for promise to resolve
    const posts = await fetchUserPosts(userData.id);
    const comments = await fetchPostComments(posts[0].id);
    
    return { userData, posts, comments };
  } catch (error) {
    console.error("Error fetching data:", error);
    throw error;
  }
}

// Using async function
fetchUserData()
  .then(data => console.log("All data:", data))
  .catch(error => console.error("Error in async function:", error));
```
Reason: Async/await was introduced to make promise-based code easier to write and read. The async keyword declares a function that implicitly returns a promise, while the await keyword pauses execution until a promise resolves, without blocking the main thread. This approach reduces nesting, improves error handling through try/catch blocks, and makes asynchronous code flow more like synchronous code.
### How do you handle errors in async/await?
Errors in async/await are handled using traditional try/catch blocks:
```js
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Error fetching data:', error);
    // Handle error (retry, fallback, user notification, etc.)
    throw error; // Re-throw or return default value
  } finally {
    // Clean up resources if needed
    console.log('Fetch operation complete');
  }
}
```
Reason: Try/catch blocks in async functions can catch both synchronous errors and promise rejections from awaited operations, providing a unified error handling approach. This makes error handling more intuitive compared to promise chains with .catch() methods. The finally block executes regardless of whether an error occurred, making it useful for cleanup operations.

### Difference between microtasks and macrotasks?
Macrotasks include setTimeout, setInterval, setImmediate, requestAnimationFrame, and I/O operations.
Microtasks include Promise callbacks, queueMicrotask, MutationObserver, and process.nextTick (in Node.js).
```js
console.log('Script start'); // 1

setTimeout(() => {
  console.log('Timeout'); // 4 (macrotask)
}, 0);

Promise.resolve()
  .then(() => console.log('Promise 1')); // 2 (microtask)

queueMicrotask(() => {
  console.log('Microtask'); // 3 (microtask)
});

console.log('Script end'); // 5

// Output:
// Script start
// Script end
// Promise 1
// Microtask
// Timeout
```
Reason: JavaScript uses different queue priorities when processing tasks. Microtasks have higher priority and are processed after the current script execution but before rendering and the next macrotask. All microtasks are processed until the queue is empty, while macrotasks are processed one per event loop iteration. This prioritization ensures promise callbacks run as soon as possible, creating more predictable behavior for chained promises.

### What is the event loop?
The event loop is JavaScript's mechanism for executing code, handling events, and processing asynchronous operations using a call stack, callback queues, and the event loop itself.
```js
console.log("Start"); // 1

setTimeout(() => {
  console.log("Timeout"); // 4
}, 0);

Promise.resolve()
  .then(() => console.log("Promise")); // 3

console.log("End"); // 2

// Output:
// Start
// End
// Promise
// Timeout
```
Reason: JavaScript's single-threaded nature requires a mechanism to handle asynchronous operations without blocking execution. The event loop continuously checks:
    - If the call stack is empty
    - If there are microtasks to process (processes all of them)
    - If there are macrotasks to process (processes one per iteration)
    - If there are rendering tasks to process
This enables non-blocking I/O operations while maintaining JavaScript's single-threaded execution model.
### Difference between setTimeout and setImmediate?              
setTimeout(callback, 0) schedules a callback to run after a minimum delay (typically 4ms in browsers even when set to 0).
setImmediate() (in Node.js) schedules a callback to execute in the next iteration of the event loop, after I/O events but before timers.
```js
// Node.js example
console.log("Start");

setTimeout(() => {
  console.log("setTimeout");
}, 0);

setImmediate(() => {
  console.log("setImmediate");
});

console.log("End");

// Output can vary, but typically:
// Start
// End
// setTimeout (or setImmediate)
// setImmediate (or setTimeout)
```
Reason: While both execute callbacks asynchronously, they have different priorities in the event loop. setTimeout(fn, 0) places the callback in the timer queue with at least a 4ms delay in browsers. setImmediate (in Node.js) places the callback in the check queue, which runs after I/O callbacks but before timers in certain conditions. The execution order can be inconsistent, but setImmediate is generally more efficient for scheduling tasks immediately after the current event loop iteration in Node.js.
### How does fetch work in JavaScript?
The Fetch API provides an interface for making network requests and returns Promises representing the response.
```js
fetch('https://api.example.com/data')
  .then(response => {
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    return response.json(); // Also returns a promise
  })
  .then(data => {
    console.log('Data:', data);
  })
  .catch(error => {
    console.error('Fetch error:', error);
  });

// Using async/await
async function fetchData() {
  try {
    const response = await fetch('https://api.example.com/data');
    if (!response.ok) {
      throw new Error(`HTTP error: ${response.status}`);
    }
    const data = await response.json();
    return data;
  } catch (error) {
    console.error('Fetch error:', error);
    throw error;
  }
}
```
- Reason: Fetch was designed as a modern replacement for XMLHttpRequest, using Promises for cleaner asynchronous code. Key points about fetch:
    - It returns a Promise that resolves to a Response object
    - It doesn't reject on HTTP error status (only on network failures)
    - It doesn't send cookies by default (unless credentials option is set)
    - Response body methods (.json(), .text(), etc.) return promises
    - It supports features like request/response streaming, CORS, and advanced request configuration
