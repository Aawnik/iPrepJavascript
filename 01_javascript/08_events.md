## Events
Events are actions or occurrences that happen in the browser, such as a user clicking a button, pressing a key, or a page finishing loading. JavaScript can detect and respond to these events.

### Event handlers
```js
    // 1. Using addEventListener (recommended)
    element.addEventListener('click', function(event) {
        console.log('Element clicked!');
    });

    // 2. Using on-event properties
    element.onclick = function(event) {
        console.log('Element clicked!');
    };

    // 3. HTML attribute (not recommended)
    // <button onclick="handleClick()">Click me</button>
    function handleClick() {
        console.log('Button clicked!');
    }
```
### Event Propagation
- Event Bubbling and capturing
- Events propagate in 2 phases
    - Capturing, Events travel from `window` down to the target element
    - Bubbling,  Events bubble up from the target element back to the `window`
    ```js
        // Third parameter specifies capturing phase (default is false - bubbling)
        element.addEventListener('click', handler, true);  // Capturing phase
        element.addEventListener('click', handler, false); // Bubbling phase (default)
    ```
    - Stopping Propagation
    ```js
        element.addEventListener('click', function(event) {
        event.stopPropagation(); // Stops event from bubbling up
        console.log('This event won\'t propagate further');
        });
    ```
- KeyDifferences
    - event.stopPropagation Vs event.preventDefault()
        - stopPropagation(): Stops the event from bubbling up or capturing down through the DOM
        - preventDefault(): Prevents the default browser behavior for the event (like form submission or link navigation)
    - inlineHandlers Vs addEventListner
        - Inline handlers: Only one per element, overwrite each other, mix HTML and JS
        - addEventListener: Multiple handlers per event type, better separation of concerns, more features

### Event Object
```js
    element.addEventListener('click', function(event) {
    console.log(event.type);          // "click"
    console.log(event.target);        // Element that triggered the event
    console.log(event.currentTarget); // Element that has the listener attached
    console.log(event.clientX);       // X coordinate of mouse click
    console.log(event.clientY);       // Y coordinate of mouse click
    
    event.preventDefault();  // Prevent default behavior (e.g., form submission)
});
```

### Event Delegation
Event delegation is a technique of attaching a single event listener to a parent element to handle events for all matching children, including future ones.
```js
    // Instead of adding event listeners to each button
    document.getElementById('buttonContainer').addEventListener('click', function(event) {
        if (event.target.tagName === 'BUTTON') {
            console.log('Button clicked:', event.target.textContent);
        }
    });
```

### Event types
- Mouse events: click, dblclick, mousedown, mouseup, mousemove, mouseover, mouseout
- Keyboard events: keydown, keyup, keypress
- Form events: submit, change, focus, blur, input
    ```js
        // Fires when form is submitted
        document.querySelector('form').addEventListener('submit', function(event) {
            event.preventDefault(); // Prevent form from submitting normally
            console.log('Form submitted');
        });

        // Fires when input value changes (after blur)
        document.querySelector('input').addEventListener('change', function() {
            console.log('Input value changed to:', this.value);
        });

        // Fires on every keystroke or input change
        document.querySelector('input').addEventListener('input', function() {
            console.log('Input value changing:', this.value);
        });
    ```
- Document/Window events: load, DOMContentLoaded, resize, scroll
    ```js
        // Fires when HTML is fully parsed (doesn't wait for stylesheets, images)
        document.addEventListener('DOMContentLoaded', function() {
            console.log('DOM fully loaded and parsed');
        });

        // Fires when the whole page has loaded (including images, stylesheets)
        window.addEventListener('load', function() {
            console.log('Page fully loaded');
        });
    ```
- Touch events: touchstart, touchmove, touchend

### Example
```js
document.addEventListener('DOMContentLoaded', function() {
    // Get elements
    const btn = document.getElementById('myButton');
    const output = document.getElementById('output');
    const form = document.getElementById('myForm');
    
    // Button click event
    btn.addEventListener('click', function(event) {
        output.textContent = 'Button clicked at: ' + new Date().toLocaleTimeString();
    });
    
    // Form submission
    form.addEventListener('submit', function(event) {
        event.preventDefault();
        const input = this.querySelector('input[type="text"]');
        output.textContent = 'Form submitted with value: ' + input.value;
    });
    
    // Keyboard event on document
    document.addEventListener('keydown', function(event) {
        if (event.key === 'Escape') {
            output.textContent = 'Escape key pressed!';
        }
    });
});
```

<hr><hr>

### What is event bubbling?
- Event bubbling is the process where an event triggered on a nested element "bubbles up" through its ancestor elements in the DOM tree.
- Explanation:
    - When an event occurs on an element, it first runs handlers on that element, then on its parent, then all the way up to the root document.
    - Events propagate from the target element upward to the document root.
    - It's the default behavior for most events.
    ```js
        // Example demonstrating bubbling
        document.querySelector("button").addEventListener("click", function() {
          console.log("Button handler");
        });

        document.querySelector("div").addEventListener("click", function() {
          console.log("Div handler");
        });

        document.body.addEventListener("click", function() {
          console.log("Body handler");
        });

        // When button is clicked, the console will show:
        // "Button handler"
        // "Div handler" 
        // "Body handler"
    ```
- REASON:_Event bubbling exists to allow for efficient event handling. Instead of attaching handlers to every element, you can catch events as they bubble up through the document.

### What is event capturing?
- Event capturing is the first phase of event propagation where the event travels from the document's root to the target element before bubbling begins.
- Explanation:
    - It's the opposite direction of bubbling.
    - Events travel from document → html → body → ... → target element.
    - It happens before bubbling but is usually invisible unless specifically enabled.
    - To use capturing, specify true as the third parameter in `addEventListener`.
    ```js
        document.querySelector("div").addEventListener("click", function() {
        console.log("Div handler - capturing phase");
        }, true); // true enables capturing phase

        document.querySelector("button").addEventListener("click", function() {
          console.log("Button handler - bubbling phase");
        });

        // When button is clicked, the console will show:
        // "Div handler - capturing phase" (parent element, capturing phase)
        // "Button handler - bubbling phase" (target element, bubbling phase)
    ```
- REASON:_Capturing exists as part of the W3C event model to provide complete control over event handling sequence. It's useful when you need to handle an event before it reaches its target.
### Difference between event.stopPropagation() and event.preventDefault()?
- stopPropagation():
    - Stops the event from traveling through the DOM (either up in bubbling or down in capturing)
    - Does not affect the default behavior of the element
    - Use when you want to prevent parent elements from receiving the event
- preventDefault():
    - Prevents the default browser behavior associated with the event
    - Does not stop the event from propagating through the DOM
    - Use when you want to override browser's default action but still allow the event to bubble
```js
    // stopPropagation example
    child.addEventListener("click", function(event) {
      event.stopPropagation();
      console.log("Child clicked - parents won't receive this event");
    });

    // preventDefault example
    document.querySelector("form").addEventListener("submit", function(event) {
      event.preventDefault();
      console.log("Form submission prevented, but event still bubbles");
    });
```
- REASON:_These methods serve different purposes: one controls event flow through the DOM (propagation), while the other controls default browser behaviors triggered by events.
### Difference between inline event handlers and addEventListener?
- Inline event handlers:
    - Written directly in HTML: `<button onclick="handleClick()">Click me</button>`
    - Only one handler per event type per element (newer ones override older ones)
    - Mix HTML with JavaScript (poor separation of concerns)
    - Executed in global scope
    - Cannot be easily removed
- addEventListener:
    - Written in JavaScript: `element.addEventListener("click", handleClick)`
    - Multiple handlers for the same event type on the same element
    - Better separation of HTML and JavaScript
    - Can specify capturing phase
    - Can be easily removed with `removeEventListener`
- REASON:_`addEventListener` provides more flexibility, better maintainability, and follows modern best practices for separation of concerns compared to inline handlers.
### What is event delegation?
- Event delegation is a technique of attaching a single event listener to a parent element that will fire for all descendants matching a selector, present and future.
- Explanation:
    - Instead of attaching events to individual child elements, attach one to their common parent
    - Use event.target to determine which child element triggered the event
    - Works because of event bubbling
    - Especially useful for dynamically added elements
    ```js
        // Without delegation (problematic for dynamically added items)
        document.querySelectorAll(".list-item").forEach(item => {
          item.addEventListener("click", handleClick);
        });

        // With delegation (works for current AND future items)
        document.getElementById("list").addEventListener("click", function(event) {
          if (event.target.classList.contains("list-item")) {
            handleClick(event);
          }
        });
    ```
- REASON:_Event delegation improves performance by reducing the number of event listeners and handles dynamically added elements without additional code.

### How do keyboard events work in JavaScript?
- Keyboard events are triggered when users interact with the keyboard, with three main types:
- keydown:
    - Fires when a key is pressed down
    - Fires repeatedly if key is held down
    - Fires before the character is inserted
- keyup:
    - Fires when a key is released
    - Fires only once per key press
- keypress: (deprecated)
    - Fires only for keys that produce character input
    - Doesn't fire for keys like Shift, Alt, arrows
```js
    document.addEventListener("keydown", function(event) {
      console.log("Key down:", event.key, event.code);
      // event.key: character representation ("a", "A", etc.)
      // event.code: physical key position ("KeyA" regardless of "a" or "A")
    
      if (event.key === "Enter") {
        // Do something when Enter key is pressed
      }
    
      // Check for combinations
      if (event.ctrlKey && event.key === "s") {
        event.preventDefault(); // Prevent browser's save dialog
        console.log("Ctrl+S pressed");
      }
    });
```
REASON:_Different keyboard events provide developers with precise control over different aspects of keyboard interaction, allowing for complex keyboard shortcuts and controls.
### Difference between input change and input events?
- change event:
    - Fires when the element loses focus after its value changed
    - For text inputs, fires only when user finishes typing and moves focus away
    - Less frequent firing (once per completed change)
    - For checkboxes/radios, fires immediately when selected
- input event:
    - Fires immediately each time the element's value changes
    - For text inputs, fires on every keystroke
    - More frequent firing (real-time feedback)
    - Better for instant validation or live previews
```js
    // Change event example
    document.querySelector("input").addEventListener("change", function() {
      console.log("Input changed and blur:", this.value);
      // Fires when user types and then clicks elsewhere
    });

    // Input event example
    document.querySelector("input").addEventListener("input", function() {
      console.log("Input changing:", this.value);
      // Fires on every character typed
    });
```
- REASON:_These events serve different purposes: `input` for real-time feedback during typing, and `change` for validations that should occur after a complete edit.

### What is DOMContentLoaded event?
- DOMContentLoaded is an event that fires when the initial HTML document has been completely loaded and parsed, without waiting for stylesheets, images, and subframes to finish loading.
- Explanation:
    - Fires earlier than the load event
    - Allows JavaScript to run as soon as the DOM structure is ready
    - Does not wait for external resources like images
    - Essential for DOM manipulation scripts
```js
    // Using DOMContentLoaded
    document.addEventListener("DOMContentLoaded", function() {
      console.log("DOM fully loaded and parsed");
      // Safe to access and manipulate DOM elements here
      document.getElementById("myElement").textContent = "DOM is ready!";
    });

    // Using load (waits for everything)
    window.addEventListener("load", function() {
      console.log("Page fully loaded, including all images and styles");
    });
```
- REASON:_`DOMContentLoaded`, allows JavaScript to run earlier in the page lifecycle than the `load` event, improving perceived performance and user experience.
### What does this refer to inside an event listener?
- Inside a regular function event handler, this refers to the DOM element that the event listener is attached to (the event's currentTarget).
- Explanation:
    - With function declarations or expressions, `this` is the element with the listener
    - With arrow functions, `this` is inherited from the surrounding scope (not the element)
    - `this` is different from `event.target`, which is the element that triggered the event
```js
    // Regular function - this is the button
    document.querySelector("button").addEventListener("click", function(event) {
      console.log(this);           // The button element
      console.log(event.target);   // The element that was actually clicked
      console.log(event.currentTarget); // Same as this (the button)
    });

    // Arrow function - this is inherited from surrounding scope
    document.querySelector("button").addEventListener("click", (event) => {
      console.log(this);           // Window object (or parent scope's this)
      console.log(event.target);   // The element that was actually clicked
      console.log(event.currentTarget); // The button element
    });
```
- REASON:_This behavior exists because of how JavaScript function context binding works. Regular functions get `this` from how they're called, while arrow functions inherit `this` lexically.

### Can you remove an event listener?
- Yes, event listeners can be removed using the `removeEventListener()` method.
- Explanation
    - Must provide the same event type, handler function, and options used when attaching
    - Anonymous functions or arrow functions cannot be removed directly
    - Function references must be stored in variables to remove them later
```js
    // Correct way to add and remove event listener
    function clickHandler() {
      console.log("Button clicked");
    }
    const button = document.querySelector("button");
    
    // Add event listener
    button.addEventListener("click", clickHandler);

    // Remove event listener when needed
    button.removeEventListener("click", clickHandler);

    // Won't work (anonymous function)
    button.addEventListener("click", function() {
      console.log("Can't remove this easily");
    });

    // Solution: store reference
    const anotherHandler = function() {
      console.log("This can be removed");
    };
    button.addEventListener("click", anotherHandler);
    button.removeEventListener("click", anotherHandler);
```
- REASON:_Removing event listeners is crucial for preventing memory leaks, especially in single-page applications or when elements are being removed from the DOM.







