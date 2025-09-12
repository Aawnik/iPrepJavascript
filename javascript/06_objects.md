## Objects
object is a collection of key-value pairs (properties and methods) used to store and organize data.
```js
let person = {
  name: "Aawni",
  age: 25,
  isDeveloper: true,
  greet: function() {
    return "Hello, " + this.name;
  }
};
```
### Ways to create the Objects
- Object Literal
    - `let car = { brand: "Tesla", model: "X" }; `
- Using **`new Object()`**
    ```js
        let user = new Object();
        user.name = "Sam";

- contructor function
    ```js
        function Person(name, age) {
          this.name = name;
          this.age = age;
        }
        let p1 = new Person("Aawni", 25);

- using class(ES_6+)
    ```js
        class Animal {
          constructor(type) {
            this.type = type;
          }
        }
        let dog = new Animal("Dog");

- using Object.create()
    ```js
        let prototypeObj = { role: "Admin" };
        let user1 = Object.create(prototypeObj);
        console.log(user1.role); // Admin (inherited)

### Accessing properties
- Dot notation
    - `console.log(person.name);`
- Bracket notation
    ```js
        console.log(person["age"]);
        let key = "isDeveloper";
        console.log(person[key]);
### deleting Properties
- ` delete person.isStudent;`

### Object Methods
- Functions being stored as object properties
    ```js
        person.greet = function() {
          return "Hello, " + this.name;
        };

### Modifying Objects
```js
person.age = 26;        // update
person.city = "Delhi";  // add
delete person.isDeveloper; // remove
```

### Object Methods
- Object.keys(obj) → returns array of keys
- Object.values(obj) → returns array of values
- Object.entries(obj) → returns array of `[key, value]` pairs
- Object.assign(target, source) → copies properties
- Object.freeze(obj) → makes object immutable
- Object.seal(obj) → allows modifying values but not adding/removing keys
```js
    const car = {
      brand: "Toyota",
      model: "Camry",
      year: 2020,
      start() {
        console.log("Engine started");
      }
    };

    car.start(); // Engine started
    console.log(Object.keys(car)); // ['brand', 'model', 'year', 'start']
```

### Objects vs Arrays
- Arrays → ordered list (index-based).
- Objects → unordered collection (key-based).

### Iterating Objects
```js
for (let key in person) {
  console.log(key, person[key]);
}
Object.keys(person).forEach(k => console.log(k, person[k]));
```
### Nested Objects
- Objects can contain other objects.
    ```js
    let user = {
      name: "Aawni",
      address: {
        city: "Delhi",
        pincode: 110001
      }
    };
    console.log(user.address.city);

### Refrence types
- Objects are reference types; assigning or passing them copies the reference, not the value.


<hr><hr>

### Difference between dot notation and bracket notation?
- Dot Notation:
    - Syntax: object.property
    - More concise and readable
    - Cannot use variables as property names
    - Cannot use property names that contain spaces, hyphens, or start with numbers
    - Cannot use reserved keywords as property names
- Bracket Notation:
    - Syntax: object['property']
    - Can use variables as property names
    - Can use property names with spaces, special characters, or that start with   numbers
    - Can use reserved keywords as property names
```js
    const user = {
      name: 'John',
      'user-age': 30
    };

    // Dot notation
    console.log(user.name);  // "John"

    // Bracket notation
    console.log(user['name']);  // "John"
    console.log(user['user-age']);  // 30 (can't use dot notation here)

    // Using variables with bracket notation
    const key = 'name';
    console.log(user[key]);  // "John"
```
- REASON:_Dot notation is preferred for its readability, but bracket notation provides flexibility when working with dynamic property names or names that aren't valid identifiers.

### How do you clone an object?
- Shallow Clone methods
    - Object.assign()
        ```js
        const original = { a: 1, b: 2 };
        const clone = Object.assign({}, original);
    - spread Operator()
        ```js
        const original = { a: 1, b: 2 };
        const clone = { ...original };
- DeepClone methods
    - JSON method
        - works for JSON safe objects
            ```js
            const original = { a: 1, b: { c: 2 } };
            const deepClone = JSON.parse(JSON.stringify(original));
    - structured clone()
        ```js
        const original = { a: 1, b: { c: 2 } };
        const deepClone = structuredClone(original);
- REASON:_Objects are reference types in JavaScript. Simple assignment (=) only copies the reference, not the actual object. Cloning creates a new object with the same properties but a different reference.
### How do you freeze an object?
- `Object.freeze(obj)`, used to make an object immutable
    ```js
        const user = { name: "John", age: 30 };
        Object.freeze(user);
        // These will fail in strict mode or silently fail in non-strict mode
        user.age = 31;      // No effect
        user.city = "NYC";  // No effect
        delete user.name;   // No effect
        console.log(user);  // { name: "John", age: 30 }
- REASON:_Freezing prevents adding, removing, or modifying properties, making objects truly immutable. This is useful for ensuring data integrity in critical operations.
### What does Object.seal() do?
- Object.seal(obj) prevents adding new properties or deleting existing ones, but allows modifying existing properties:
    ```js
        const user = { name: "John", age: 30 };
        Object.seal(user);

        user.age = 31;      // Works! (modifying existing property)
        user.city = "NYC";  // No effect (adding new property)
        delete user.name;   // No effect (deleting property)

        console.log(user);  // { name: "John", age: 31 }
        console.log(Object.isSealed(user));  // true
- REASON:_Sealing is a middle ground between no protection and full freezing. It's useful when you want to maintain the structure of an object while allowing updates to existing values.
### What is Object.assign()?
- `Object.assign(target, ...sources)`, copies all enumerable own properties from source object(s) to a target object and returns the target object:
    ```js
    // Clone an object
    const original = { a: 1, b: 2 };
    const copy = Object.assign({}, original);

    // Merge objects
    const o1 = { a: 1 };
    const o2 = { b: 2 };
    const o3 = { c: 3 };
    const merged = Object.assign({}, o1, o2, o3);
    console.log(merged); // { a: 1, b: 2, c: 3 }

    // Properties are overwritten by later sources
    const target = { a: 1, b: 2 };
    const source = { b: 3, c: 4 };
    Object.assign(target, source);
    console.log(target); // { a: 1, b: 3, c: 4 }
- REASON:_`Object.assign()`, provides a way to combine multiple objects and create copies. It performs shallow copying (nested objects are still shared).
### How do you check if a property exists in an object?
- using `in` operator
    ```js
    const user = { name: "John", age: 30 };
    console.log("name" in user);  // true
    console.log("city" in user);  // false
- using `hasOwnProperty` method
    ```js
    console.log(user.hasOwnProperty("name"));  // true
    console.log(user.hasOwnProperty("toString"));  // false (not own property)
- comparing with undefined
    ```js
    console.log(user.name !== undefined);  // true
    console.log(user.city !== undefined);  // false
- REASON:_The choice depends on whether you need to check inherited properties (`in`), own properties (`hasOwnProperty()`), or just want a quick check that might have edge cases (`!== undefined`).
### Difference between enumerable and non-enumerable properties?
- Enumerable properties:
    - Show up in for...in loops and Object.keys()
    - Typically properties created by normal assignment
- Non-enumerable properties:
    - Don't appear in for...in loops or Object.keys()
    - Often built-in methods and properties like toString()
    - Can be created using Object.defineProperty()
```js
    const user = { name: "John" };

    // Define a non-enumerable property
    Object.defineProperty(user, "id", {
      value: 12345,
      enumerable: false
    });

    console.log(user.id);  // 12345
    console.log(Object.keys(user));  // ["name"] (id is not included)

    for (let key in user) {
      console.log(key);  // Only logs "name", not "id"
    }
```
- Reason:_Enumerability controls visibility during property iteration, allowing you to hide certain properties from common enumeration mechanisms while keeping them accessible directly.

### What does Object.keys() return?
- Object.keys(obj) returns an array of a given object's own enumerable property names (keys):
- This method provides a way to get all the enumerable keys of an object as an array, useful for iteration and inspection of object structure.
```js
    const person = { 
      name: "Alice", 
      age: 25,
      greet() { return "Hello"; }
    };

    console.log(Object.keys(person));  // ["name", "age", "greet"]

    // Non-enumerable properties are not included
    Object.defineProperty(person, "id", {
      value: 12345,
      enumerable: false
    });

    console.log(Object.keys(person));  // ["name", "age", "greet"] (id not included)
```

### What is a computed property?
- A computed property is a property name that is determined dynamically using an expression in square brackets.
- Computed properties allows dynamic property names in object literals, making it possible to create flexible object structures based on variables or expressions.
```js
    // Basic computed property
    const key = "name";
    const user = {
      [key]: "John"  // Property name is the value of key variable
    };
    console.log(user.name);  // "John"

    // More complex expression
    const prefix = "user";
    const obj = {
      [`${prefix}_id`]: 12345,
      [`${prefix}_name`]: "John"
    };
    console.log(obj.user_id);  // 12345
    console.log(obj.user_name);  // "John"
```

### How do you delete an object property?
- using `delete` property, to remove the property from an object
```js
    const user = {
      name: "John",
      age: 30,
      city: "New York"
    };

    delete user.city;
    console.log(user);  // { name: "John", age: 30 }

    // Using bracket notation
    delete user["age"];
    console.log(user);  // { name: "John" }

    // Delete returns true for existing and non-existing properties
    console.log(delete user.name);  // true
    console.log(delete user.country);  // true (even though it doesn't exist)
```
- NOTE:_delete cannot remove
    - Properties declared with var, let, or const
    - Built-in properties like Math.PI
    - Properties with configurable: false
- REASON:_`delete`, allows dynamic removal of object properties at runtime, enabling flexible object manipulation.