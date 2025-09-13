## JSON

- JSON (JavaScript Object Notation) is a lightweight data-interchange format that's easy for humans to read and write, and easy for machines to parse and generate.
- What is JSON?
- JSON is a text format that is language-independent but uses conventions familiar to programmers of the C-family of languages. It's often used to transmit data between a server and web application as an alternative to XML.
- `const jsonString = '{"name":"John","age":30,"city":"New York"}';`

### JSON Syntax

    - Data is represented in name/value pairs
    - Curly braces {} hold objects
    - Square brackets [] hold arrays
    - Names must be strings in double quotes
    - Values can be:
        - Strings (in double quotes)
        - Numbers
        - Objects
        - Arrays
        - Booleans (true or false)
        - null

### Key JSON Methods in JavaScript

- JSON.parse(), Converts a JSON string into a JavaScript object:

```js
const jsonString = '{"name":"John","age":30,"city":"New York"}';
const obj = JSON.parse(jsonString);
console.log(obj.name); // John
```

- JSON.stringify(), Converts a JavaScript object into a JSON string:

```js
const person = {
  name: "John",
  age: 30,
  city: "New York",
};
const jsonString = JSON.stringify(person);
console.log(jsonString); // {"name":"John","age":30,"city":"New York"}
```

- Pretty-printing JSON

```js
const person = {
  name: "John",
  age: 30,
  address: {
    street: "123 Main St",
    city: "New York",
  },
};
// The third parameter (2) specifies the number of spaces for indentation
console.log(JSON.stringify(person, null, 2));
```

### Limitations

- JSON doesn't support functions
- JSON doesn't support undefined values
- JSON doesn't support Date objects (they're converted to strings)
- JSON keys must be in double quotes
- JSON doesn't handle circular references

```js
// Functions and undefined are lost in conversion
const obj = {
  name: "John",
  greet: function () {
    return "Hello";
  },
  data: undefined,
};
console.log(JSON.stringify(obj)); // {"name":"John"}
```

### Working with nested Objects

````js
    const data = {
      users: [
        { id: 1, name: "Alice", roles: ["admin", "user"] },
        { id: 2, name: "Bob", roles: ["user"] }
      ],
      settings: {
        darkMode: true,
        notifications: {
          email: false,
          push: true
        }
      }
    };

    // Converting to JSON
    const jsonData = JSON.stringify(data);

    // Parsing back
    const parsedData = JSON.parse(jsonData);
    console.log(parsedData.users[0].name); // Alice
    console.log(parsedData.settings.notifications.push); // true
    ```
    ### Handling Circular References
    ```js
    const circular = { name: "Circular Object" };
    circular.self = circular; // Creates a circular reference

    try {
      JSON.stringify(circular);
    } catch (error) {
      console.error("Cannot stringify circular references:", error.message);
    }

    // Solution: Custom replacer function
    function replacer() {
      const cache = new Set();
      return (key, value) => {
        if (typeof value === 'object' && value !== null) {
          if (cache.has(value)) {
            return "[Circular]";
          }
          cache.add(value);
        }
        return value;
      };
    }

    console.log(JSON.stringify(circular, replacer()));
````

### Storage with JSON

```js
// Storing in localStorage
const user = {
  name: "John",
  preferences: {
    theme: "dark",
    fontSize: "medium",
  },
  lastLogin: new Date(),
};

// Save to localStorage
localStorage.setItem("user", JSON.stringify(user));

// Retrieve from localStorage
const storedUser = JSON.parse(localStorage.getItem("user"));
console.log(storedUser);
// Note: The Date object is now a string
```

<hr><hr>

### What is JSON?

- JSON (JavaScript Object Notation) is a lightweight data-interchange format that's easy for humans to read and write, and easy for machines to parse and generate.

- Reason: JSON was created as a language-independent data format that's simpler than XML but still structured enough for data interchange. It's based on JavaScript object literal syntax but is used across many programming languages as a universal data format for APIs, configuration files, and data storage.
3
### How do you parse JSON in JavaScript?

- using the JSON.parse() method:
- Parsing is necessary because JSON data is typically received as a string (from APIs, files, etc.) and needs to be converted into JavaScript objects before you can work with it programmatically. The JSON.parse() method handles this conversion, transforming the text representation into usable JavaScript objects with properties and values.

```js
const jsonString = '{"name":"John","age":30,"city":"New York"}';
const obj = JSON.parse(jsonString);
console.log(obj.name); // "John"
```

### How do you stringify an object into JSON?

- using the JSON.stringify() method:
- Stringification is required when you need to send JavaScript objects to a server, store them in localStorage, or transmit them across systems. Since JavaScript objects can only exist within a JavaScript environment, they must be converted to a text-based format (JSON) for storage or transmission.

```js
const person = {
  name: "John",
  age: 30,
  city: "New York",
};
const jsonString = JSON.stringify(person);
console.log(jsonString); // '{"name":"John","age":30,"city":"New York"}'
```

### Can JSON store functions?

- No, JSON cannot store functions.

```js
const obj = {
  name: "John",
  greet: function () {
    return "Hello";
  },
};
console.log(JSON.stringify(obj)); // '{"name":"John"}'
```

- JSON is a data-interchange format designed for structured data only, not executable code. Functions are omitted during stringification for several reasons:
  - Security: Including executable code in data exchanges would create significant security risks
  - Cross-language compatibility: Other languages couldn't interpret JavaScript functions
  - JSON specification: The official JSON spec explicitly excludes function

### Difference between JSON.stringify and JSON.parse?

- JSON.stringify(): Converts JavaScript objects to JSON strings (serialization)
- JSON.parse(): Converts JSON strings to JavaScript objects (deserialization)

```js
// Stringify (object → string)
const obj = { name: "John", age: 30 };
const jsonString = JSON.stringify(obj);
console.log(typeof jsonString); // "string"

// Parse (string → object)
const parsedObj = JSON.parse(jsonString);
console.log(typeof parsedObj); // "object"
```

- These methods complement each other in the data lifecycle: stringify prepares data to leave the JavaScript environment, while parse prepares received data to be used within JavaScript. They're essential for data persistence and communication between systems.

### What is JSON schema?

- JSON Schema is a vocabulary that allows you to annotate and validate JSON documents. It describes the structure, constraints, and validation rules that a JSON document should follow.

```js
// Example JSON Schema
const schema = {
  type: "object",
  properties: {
    name: { type: "string" },
    age: { type: "integer", minimum: 0 },
    email: { type: "string", format: "email" },
  },
  required: ["name", "email"],
};
```

- JSON Schema provides a way to validate the structure and content of JSON data, ensuring it meets expected requirements before processing. This is especially important in APIs and data exchange systems where malformed data could cause errors or security issues.

### Can JSON keys have spaces?

Yes, JSON keys can have spaces, but they must be enclosed in double quotes:

- JSON specification requires all keys to be strings enclosed in double quotes, regardless of whether they contain spaces. This ensures consistent parsing and avoids ambiguity that could arise from unquoted keys with spaces.

```js
// Valid JSON
const json = '{"first name": "John", "last name": "Doe"}';
const person = JSON.parse(json);
console.log(person["first name"]); // "John"
```

### How do you handle circular JSON?

- By default, circular references in objects cause JSON.stringify() to throw an error. You can handle circular JSON using a custom replacer function:

```js
function replacerWithCircularHandling() {
  const seen = new WeakSet();
  return (key, value) => {
    if (typeof value === "object" && value !== null) {
      if (seen.has(value)) {
        return "[Circular]";
      }
      seen.add(value);
    }
    return value;
  };
}

const circular = { name: "Circular Object" };
circular.self = circular; // Creates a circular reference

const safeJSON = JSON.stringify(circular, replacerWithCircularHandling());
console.log(safeJSON); // '{"name":"Circular Object","self":"[Circular]"}'
```

### Difference between localStorage and sessionStorage for JSON?

- Both require JSON.stringify before storing and JSON.parse after retrieving:

```js
// localStorage example (persists across browser sessions)
localStorage.setItem("user", JSON.stringify({ name: "John" }));
const user = JSON.parse(localStorage.getItem("user"));

// sessionStorage example (only for current tab/session)
sessionStorage.setItem("preferences", JSON.stringify({ theme: "dark" }));
const preferences = JSON.parse(sessionStorage.getItem("preferences"));
```

- Key differences:
  - localStorage: Data persists until explicitly deleted, surviving browser restarts
  - sessionStorage: Data persists only for the current browser tab/window session
- Reason: Both storage options can only store strings, which is why JSON.stringify/parse are necessary. The choice between them depends on whether data should persist across browser sessions (localStorage) or be limited to the current session (sessionStorage).

### Can you store nested objects in JSON?

- Yes, JSON fully supports nested objects and arrays to arbitrary depths:
- Nested structures are a core feature of JSON, allowing complex data hierarchies to be represented. This capability is crucial for modeling real-world data relationships and organizing information in a logical structure.

```js
const data = {
  person: {
    name: "John",
    address: {
      street: "123 Main St",
      city: "New York",
      location: {
        lat: 40.7128,
        lng: -74.006,
      },
    },
    hobbies: [
      "reading",
      "hiking",
      { type: "sports", favorites: ["tennis", "basketball"] },
    ],
  },
};

const jsonString = JSON.stringify(data);
const parsedData = JSON.parse(jsonString);
console.log(parsedData.person.address.location.lat); // 40.7128
```
