## Document Object Model (DOM)
- the programming interface for web documents. It represents the page as nodes and objects that JavaScript can interact with.

### What is DOM..?
    - The DOM is a tree-like representation of your HTML document
    - It allows JavaScript to access and manipulate HTML elements and their attributes
    - Each HTML element becomes a JavaScript object that can be selected, modified, added, or removed

### selecting DOM elements
- getElementById vs QuerySelector
```js
    // getElementById - selects by ID attribute (fastest)
    const header = document.getElementById('main-header'); // Returns single element

    // querySelector - selects using CSS selectors (more flexible)
    const header = document.querySelector('#main-header'); // Returns first matching element
    const firstParagraph = document.querySelector('p'); 
    const activeItem = document.querySelector('.active');

    // querySelectorAll - returns all matching elements as NodeList
    const allParagraphs = document.querySelectorAll('p');
    const navItems = document.querySelectorAll('.nav-item');

    // getElementsByClassName - returns HTMLCollection (live)
    const items = document.getElementsByClassName('item');

    // getElementsByTagName - returns HTMLCollection (live)
    const paragraphs = document.getElementsByTagName('p');
```
- `getElementById` is faster but limited to ID selection, while `querySelector` is more versatile with CSS selector support.

### modifying Element Content
- innerText vs innerHTML
```js
    // innerText - sets or returns the text content
    element.innerText = 'Hello World';

    // innerHTML - sets or returns the HTML content (can include HTML tags)
    element.innerHTML = '<span>Hello</span> World';

    // textContent - similar to innerText but handles whitespace differently
    element.textContent = 'Hello World';
```
- `innerHTML` parses content as HTML (can create security risks with XSS), while `innerText/textContent` treat content as plain text.

### creating and adding Elements
```js
    // Create a new element
    const newDiv = document.createElement('div');

    // Add content
    newDiv.innerText = 'I am a new div';

    // Add attributes
    newDiv.setAttribute('class', 'highlight');
    // Alternative: newDiv.className = 'highlight';
    // Alternative: newDiv.classList.add('highlight');

    // Append to the DOM
    document.body.appendChild(newDiv);

    // Insert before another element
    const referenceElement = document.getElementById('existing-element');
    document.body.insertBefore(newDiv, referenceElement);

    // Modern insertion methods
    parentElement.append(newDiv);       // Adds at end, allows multiple elements
    parentElement.prepend(newDiv);      // Adds at beginning
    referenceElement.before(newDiv);    // Adds before reference
    referenceElement.after(newDiv);     // Adds after reference
```
### removing Elements
```js
    // Remove an element (modern)
    element.remove();

    // Remove an element (older browsers)
    element.parentElement.removeChild(element);

    // Remove all children of an element
    while (element.firstChild) {
      element.removeChild(element.firstChild);
    }
    // Alternative: element.innerHTML = '';
```
### Modifying CSS styles
```js
    // Direct style manipulation
    element.style.color = 'blue';
    element.style.backgroundColor = 'lightgray'; // Note camelCase for CSS properties

    // Multiple styles at once
    Object.assign(element.style, {
      color: 'white',
      backgroundColor: 'black',
      fontSize: '16px'
    });

    // Adding/removing CSS classes
    element.classList.add('active');
    element.classList.remove('disabled');
    element.classList.toggle('highlight');
    element.classList.replace('old-class', 'new-class');
    element.classList.contains('active'); // Check if class exists
```
### Element Traversal
- childNodes vs children
    - childNodes includes text nodes (like whitespace) and comment nodes, while children includes only actual HTML elements.
    ```js
        // childNodes - returns all child nodes (including text nodes and comments)
        const allNodes = element.childNodes; // Returns NodeList

        // children - returns only element nodes (HTML elements)
        const elementNodes = element.children; // Returns HTMLCollection
    ```
- parent and Sibling Navigation
```js
    // Parent nodes
    const parent = element.parentNode;  // Any parent node
    const elementParent = element.parentElement; // Only element parents

    // Siblings
    const nextSibling = element.nextSibling;         // Can be text node
    const nextElementSibling = element.nextElementSibling; // Only element nodes
    const prevSibling = element.previousSibling;     // Can be text node
    const prevElementSibling = element.previousElementSibling; // Only element nodes

    // First and last children
    const firstChild = element.firstChild;         // Can be text node
    const firstElementChild = element.firstElementChild; // Only element nodes
    const lastChild = element.lastChild;           // Can be text node
    const lastElementChild = element.lastElementChild; // Only element nodes
```
- Event Listeners
```js
    // Adding an event listener to a button
    const button = document.querySelector('#myButton');

    button.addEventListener('click', function(event) {
      console.log('Button clicked!');
      console.log(event); // The event object
    });

    // Using arrow function
    button.addEventListener('click', (e) => {
      console.log('Clicked with arrow function');
      console.log(e.target); // The element that triggered the event
    });

    // Using named function
    function handleClick(e) {
      console.log('Clicked with named function');
    }
    button.addEventListener('click', handleClick);

    // Removing event listener (only works with named functions)
    button.removeEventListener('click', handleClick);
```
- Attribute Manipulation
```js
    // Get attribute
    const href = link.getAttribute('href');

    // Set attribute
    button.setAttribute('disabled', 'true');

    // Check attribute existence
    const hasAttr = element.hasAttribute('data-id');

    // Remove attribute
    element.removeAttribute('disabled');

    // Data attributes
    const value = element.dataset.userId; // Gets data-user-id
    element.dataset.value = '123';       // Sets data-value="123"
```
### Example
```js
    // Creating a dynamic list
    function addItemToList(text) {
      // Create elements
      const li = document.createElement('li');
      const span = document.createElement('span');
      const deleteBtn = document.createElement('button');
    
      // Set content and attributes
      span.textContent = text;
      deleteBtn.textContent = 'Delete';
      deleteBtn.className = 'delete-btn';
    
      // Add event listener
      deleteBtn.addEventListener('click', function() {
        li.remove();
      });
    
      // Construct the element tree
      li.appendChild(span);
      li.appendChild(deleteBtn);
    
      // Add to DOM
      document.getElementById('todo-list').appendChild(li);
    }

    // Usage
    document.getElementById('add-btn').addEventListener('click', function() {
      const input = document.getElementById('todo-input');
      if (input.value.trim() !== '') {
        addItemToList(input.value);
        input.value = '';
      }
    });
```

<hr><hr>

### What is the DOM in JavaScript?
- The DOM (Document Object Model) is a programming interface that represents HTML documents as a tree structure where each element is a node.
REASON:_The DOM provides a way for JavaScript to interact with HTML documents by representing the document as objects that can be manipulated. It serves as a bridge between web content and programming languages.
// DOM tree example
document                 // Root node
  └── html
      ├── head
      │   ├── title
      │   └── meta
      └── body
          ├── div
          ├── p
          └── button
### Difference between document.getElementById and querySelector?
- getElementById:
    - Selects a single element by its ID attribute
    - Returns null if no match is found
    - Faster performance (optimized method)
    - Only works with IDs
- querySelector:
    - Selects the first element that matches a CSS selector
    - Returns null if no match is found
    - More flexible (works with any CSS selector)
    - Slightly slower performance
```js
    // getElementById - only works with IDs
    const header = document.getElementById('main-header');

    // querySelector - works with any CSS selector
    const header = document.querySelector('#main-header');
    const firstParagraph = document.querySelector('p');
    const activeItem = document.querySelector('.active');
```
- REASON:_getElementById when you know the element's ID for better performance. Use querySelector when you need more flexibility with different types of selectors or complex selection patterns.

### How do you change the text of an element?
- Ways to change the text
- REASON:_Choose based on your needs:
    - `textContent` for raw text manipulation, 
    - `innerText` for human-readable text respecting visibility,
    - `innerHTML` when you need to include HTML elements (but be cautious of security risks).
```js
    // Using textContent - handles all text, including script and style elements
    element.textContent = 'New text content';

    // Using innerText - only visible text, respects CSS styling
    element.innerText = 'New inner text';

    // Using innerHTML - interprets HTML tags
    element.innerHTML = 'Text with <strong>HTML</strong> tags';
```

### How do you add a new element to the DOM?
```js
    // 1. Create the element
    const newParagraph = document.createElement('p');

    // 2. Add content/attributes
    newParagraph.textContent = 'This is a new paragraph';
    newParagraph.className = 'info-text';

    // 3. Append to the DOM
    // Traditional method
    document.body.appendChild(newParagraph);

    // Modern methods (ES6+)
    document.body.append(newParagraph);  // Can append multiple nodes and text
    parentElement.prepend(newElement);   // Insert at beginning
    targetElement.before(newElement);    // Insert before target
    targetElement.after(newElement);     // Insert after target
```
- REASON:_Adding elements is a multi-step process (create, configure, insert) to ensure proper element setup before insertion. Modern methods provide more flexibility than traditional `appendChild`.
### Difference between innerText and innerHTML?
- innerText:
    - Sets or gets the visible text content of an element
    - Doesn't parse HTML tags (treats them as text)
    - Safe from XSS attacks
    - Respects CSS styling (won't show hidden elements)
- innerHTML:
    - Sets or gets the HTML content including tags
    - Parses and renders HTML tags
    - Potential security risk with XSS if user input is used
    - Can access/modify the complete HTML structure
```js
    // innerText example - HTML tags are treated as text
    element.innerText = '<strong>Bold text?</strong>';
    // Result: Displays literal "<strong>Bold text?</strong>" on page

    // innerHTML example - HTML tags are parsed
    element.innerHTML = '<strong>Bold text!</strong>';
    // Result: Displays "Bold text!" as bold text
```
- REASON:_`innerText` when you want to work with plain text without HTML parsing (safer). Use `innerHTML` when you need to insert formatted HTML content (but with caution regarding security).

### How do you remove a DOM element?
```js
    // Modern method (supported in all modern browsers)
    element.remove();

    // Traditional method (works in older browsers)
    element.parentNode.removeChild(element);

    // Remove all children
    while (parent.firstChild) {
      parent.removeChild(parent.firstChild);
    }
    // Or simpler alternative: parent.innerHTML = '';
```
- REASON:_Element removal requires detaching the node from its parent. The modern `remove()` method simplifies this process, while the traditional method requires explicit parent-child relationship handling for wider browser support.

### How do you change CSS styles with JS?
```js
    // Direct style manipulation
    element.style.color = 'blue';
    element.style.backgroundColor = 'lightgray'; // Note camelCase for properties   
    // Setting multiple styles at once
    Object.assign(element.style, {
      color: 'white',
      backgroundColor: 'black',
      fontSize: '16px'
    }); 
    // Adding/removing CSS classes (preferred approach)
    element.classList.add('active');
    element.classList.remove('disabled');
    element.classList.toggle('highlight');
    element.classList.replace('old-class', 'new-class');
```
REASON:_Direct style manipulation is useful for dynamic, calculated styles. However, using CSS classes is preferred for maintainability as it separates styling logic from JavaScript and allows for easier state management.

### Difference between childNodes and children?
- childNodes:
    - Returns all child nodes, including:
        - Element nodes
        - Text nodes (including whitespace)
        - Comment nodes
    - Returns a live NodeList
    - Often contains unexpected text nodes from whitespace
- children:
    - Returns only element nodes (HTML elements)
    - Returns a live HTMLCollection
    - Cleaner when you only need elements
```js
    const parent = document.getElementById('parent');

    // childNodes includes text nodes (whitespace) and comments
    console.log(parent.childNodes.length);  // Might be higher than expected

    // children includes only element nodes
    console.log(parent.children.length);    // Only actual HTML elements
```
REASON:_Use `children` when you only need HTML elements (most common case). Use `childNodes` when you need access to all node types, including text and comment nodes.

### How do you navigate parent and sibling elements?
```js
    // Parent navigation
    const parent = element.parentNode;       // Any parent node
    const elementParent = element.parentElement; // Only element parents    

    // Sibling navigation
    const nextSibling = element.nextSibling;         // Can be text node
    const nextElement = element.nextElementSibling;  // Only element nodes
    const prevSibling = element.previousSibling;     // Can be text node
    const prevElement = element.previousElementSibling; // Only element nodes   

    // Child navigation
    const firstChild = element.firstChild;         // Can be text node
    const firstElement = element.firstElementChild; // Only element nodes
    const lastChild = element.lastChild;           // Can be text node
    const lastElement = element.lastElementChild;  // Only element nodes
```
- REASON:_DOM traversal methods allow you to navigate the DOM tree in all directions. The methods ending with "Element" help avoid text nodes (often whitespace) and focus on actual HTML elements, which is usually what you want.

### How do you create an event listener for a button?
```js
    // Select the button
    const button = document.querySelector('#myButton');

    // Add event listener with anonymous function
    button.addEventListener('click', function(event) {
      console.log('Button clicked!');
      console.log(event); // Event object contains details about the event
    });

    // Using arrow function
    button.addEventListener('click', (e) => {
      console.log('Button clicked with arrow function');
      e.preventDefault(); // Prevent default behavior if needed
    });

    // Using named function for easy removal
    function handleClick(e) {
      console.log('Button clicked with named function');
    }
    button.addEventListener('click', handleClick);

    // Remove event listener when needed
    button.removeEventListener('click', handleClick);
```
REASON:_Event listeners allow for interactive web pages by responding to user actions. They follow an event-driven programming model, where code execution is triggered by events rather than running sequentially. Named functions are useful when you need to remove listeners later.



