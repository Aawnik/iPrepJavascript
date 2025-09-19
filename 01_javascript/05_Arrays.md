## Arrays
- the high-level list like objects which is being used to store the values in single variable.

### creating Arrays
- // Array literal (most common)
    `const fruits = ['Apple', 'Banana', 'Orange'];`

- // Array constructor
    `const numbers = new Array(1, 2, 3, 4, 5);`

- // Empty array
    `const empty = [];`

- // Array with mixed data types
    `const mixed = [1, 'Hello', true, null, {name: 'John'}, [1, 2, 3]];`

- // Creating array with specific length
    `const fixedLength = new Array(5); // Creates array with 5 empty slots`

- // Array.from() - creates array from array-like or iterable objects
    `const arrayFromString = Array.from('hello'); // ['h', 'e', 'l', 'l', 'o']`

### Accessing Array Elements
```js
    const fruits = ['Apple', 'Banana', 'Orange', 'Mango'];
    // Access by index (zero-based)
    console.log(fruits[0]); // 'Apple'
    console.log(fruits[2]); // 'Orange'

    // Last element
    console.log(fruits[fruits.length - 1]); // 'Mango'

    // Using at() method (ES2022)
    console.log(fruits.at(-1)); // 'Mango' (last element)
    console.log(fruits.at(1));  // 'Banana'
```
### Basic Array Properties & Methods
- Array Length
```js
    const numbers = [1, 2, 3, 4, 5];
    console.log(numbers.length); // 5

    // Modifying length can truncate the array
    numbers.length = 3;
    console.log(numbers); // [1, 2, 3]
```
- Adding/Removing the elements
    - Push_&_unshift
        - push() adds elements to the end of an array
        - unshift() adds elements to the beginning of an array
        - Both return the new length of the array
        - push() is generally faster than unshift() as it doesn't require reindexing elements
    ```js
        const fruits = ['Apple', 'Banana'];
        // push() - adds elements to the end, returns new length
        const newLength = fruits.push('Orange', 'Mango'); 
        console.log(fruits); // ['Apple', 'Banana', 'Orange', 'Mango']
        console.log(newLength); // 4

        // unshift() - adds elements to the beginning, returns new length
        const newLength2 = fruits.unshift('Pineapple', 'Strawberry');
        console.log(fruits); // ['Pineapple', 'Strawberry', 'Apple', 'Banana', 'Orange', 'Mango']
        console.log(newLength2); // 6
    ``` 
    - Pop_&_shift
        - pop() removes the last element from an array
        - shift() removes the first element from an array
        - Both return the removed element
        - pop() is generally faster than shift() as it doesn't require reindexing elements
    ```js
        const fruits = ['Apple', 'Banana', 'Orange', 'Mango'];
        // pop() - removes last element, returns removed element
        const lastFruit = fruits.pop();
        console.log(lastFruit); // 'Mango'
        console.log(fruits); // ['Apple', 'Banana', 'Orange']

        // shift() - removes first element, returns removed element
        const firstFruit = fruits.shift();
        console.log(firstFruit); // 'Apple'
        console.log(fruits); // ['Banana', 'Orange']
    ```
- Array Methods for Manipulation
    - slice and Splice
        - slice() is non-mutating - returns a new array without modifying original
        - splice() is mutating - modifies the original array
        - slice() takes start and end indexes
        - splice() takes start index, delete count, and items to insert
        ```js
            // slice() - returns a shallow copy of a portion of an array (non-mutating)
            const numbers = [1, 2, 3, 4, 5];
            const sliced = numbers.slice(1, 4); // start at index 1, end before index 4
            console.log(sliced); // [2, 3, 4]
            console.log(numbers); // [1, 2, 3, 4, 5] (original unchanged)

            // splice() - changes array by removing/replacing elements (mutating)
            const letters = ['a', 'b', 'c', 'd', 'e'];
            // starting at index 2, remove 2 elements, insert 'X', 'Y'
            const removed = letters.splice(2, 2, 'X', 'Y');
            console.log(removed); // ['c', 'd'] (removed elements)
            console.log(letters); // ['a', 'b', 'X', 'Y', 'e'] (original modified)
        ```
    - concat_&_join
        ```js
            // concat() - merges arrays, returns new array
            const arr1 = [1, 2];
            const arr2 = [3, 4];
            const combined = arr1.concat(arr2, [5, 6]);
            console.log(combined); // [1, 2, 3, 4, 5, 6]
            console.log(arr1); // [1, 2] (original unchanged)

            // join() - joins all elements into a string
            const elements = ['Fire', 'Air', 'Water'];
            console.log(elements.join()); // 'Fire,Air,Water'
            console.log(elements.join(' - ')); // 'Fire - Air - Water'
        ```
    - Searching_&_Sorting
        ```js
        const fruits = ['Apple', 'Banana', 'Orange', 'Mango', 'Banana'];

        // indexOf() - find first occurrence index
        console.log(fruits.indexOf('Banana')); // 1
        console.log(fruits.indexOf('Cherry')); // -1 (not found)

        // lastIndexOf() - find last occurrence index
        console.log(fruits.lastIndexOf('Banana')); // 4

        // includes() - checks if element exists
        console.log(fruits.includes('Mango')); // true

        // sort() - sorts elements in place
        const nums = [40, 1, 5, 200];
        nums.sort();
        console.log(nums); // [1, 200, 40, 5] (sorts as strings by default)

        // Numeric sort
        nums.sort((a, b) => a - b);
        console.log(nums); // [1, 5, 40, 200]

        // reverse() - reverses array in place
        fruits.reverse();
        console.log(fruits); // ['Banana', 'Mango', 'Orange', 'Banana', 'Apple']
        ```
### Array Methods
- forEach, Map, filter, reduce
    - map() returns a new array with transformed elements
    - forEach() doesn't return anything (undefined), just executes function on each element
    - map() is chainable with other methods, forEach() is not
    ```js
    const numbers = [1, 2, 3, 4, 5];
    // forEach() - executes function for each element (no return value)
    numbers.forEach(num => {
      console.log(num * 2);
    }); // logs 2, 4, 6, 8, 10

    // map() - creates new array with results of function
    const doubled = numbers.map(num => num * 2);
    console.log(doubled); // [2, 4, 6, 8, 10]

    // filter() - creates new array with elements passing test
    const evens = numbers.filter(num => num % 2 === 0);
    console.log(evens); // [2, 4]

    // reduce() - reduces array to single value
    const sum = numbers.reduce((total, current) => total + current, 0);
    console.log(sum); // 15 (0+1+2+3+4+5)

    // reduce with different initial value
    const sumPlusTen = numbers.reduce((total, current) => total + current, 10);
    console.log(sumPlusTen); // 25 (10+1+2+3+4+5)
    ```

- find, findIndex, some, every
    ```js
    const people = [
      { name: 'John', age: 25 },
      { name: 'Jane', age: 30 },
      { name: 'Jim', age: 18 }
    ];
    
    // find() - returns first element that passes test
    const jane = people.find(person => person.name === 'Jane');
    console.log(jane); // { name: 'Jane', age: 30 }
    
    // findIndex() - returns index of first element passing test
    const jimIndex = people.findIndex(person => person.name === 'Jim');
    console.log(jimIndex); // 2
    
    // some() - tests if at least one element passes test
    const anyAdults = people.some(person => person.age >= 18);
    console.log(anyAdults); // true
    
    // every() - tests if all elements pass test
    const allAdults = people.every(person => person.age >= 18);
    console.log(allAdults); // true
    ```

### Array Type Checking & Flatenning
```js
// Array.isArray() - checks if value is an array
console.log(Array.isArray([1, 2, 3])); // true
console.log(Array.isArray('hello')); // false
console.log(Array.isArray({})); // false

// flat() - flattens nested arrays
const nested = [1, 2, [3, 4, [5, 6]]];
console.log(nested.flat()); // [1, 2, 3, 4, [5, 6]] (default depth 1)
console.log(nested.flat(2)); // [1, 2, 3, 4, 5, 6] (depth 2)
console.log(nested.flat(Infinity)); // [1, 2, 3, 4, 5, 6] (all levels)

// flatMap() - combination of map() and flat()
const sentences = ['hello world', 'the quick brown fox'];
const words = sentences.flatMap(sentence => sentence.split(' '));
console.log(words); // ['hello', 'world', 'the', 'quick', 'brown', 'fox']
```

### Array Destructuring & Spread Operator
```js
// Destructuring arrays
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first); // 1
console.log(second); // 2
console.log(rest); // [3, 4, 5]

// Spread operator
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]

// Copy array with spread
const original = [1, 2, 3];
const copy = [...original];
console.log(copy); // [1, 2, 3]
```
<hr><hr>

### Difference between push and unshift?
- push:
    - Adds elements to the end of an array
    - Returns the new length of the array
    - Generally more efficient (better performance)
- unshift:
    - Adds elements to the beginning of an array
    - Returns the new length of the array
    - Less efficient because it requires re-indexing all existing elements
    ```js
        const arr = [2, 3];
        arr.push(4);     // returns 3, arr is now [2, 3, 4]
        arr.unshift(1);  // returns 4, arr is now [1, 2, 3, 4]
    ```
### Difference between pop and shift?
- pop:
    - Removes the last element from an array
    - Returns the removed element
    - Generally more efficient (better performance)
- shift:
    - Removes the first element from an array
    - Returns the removed element
    - Less efficient because it requires re-indexing all remaining elements
```js
const arr = [1, 2, 3, 4];
const last = arr.pop();   // returns 4, arr is now [1, 2, 3]
const first = arr.shift(); // returns 1, arr is now [2, 3]
```
### How does splice work?
- Modifies an array in-place by removing, replacing, or adding elements
- Syntax: array.splice(startIndex, deleteCount, item1, item2, ...)
- Returns an array containing the deleted elements
- Mutates the original array
```js
const months = ['Jan', 'March', 'April', 'June'];
// Insert at index 1, delete 0 elements, insert 'Feb'
months.splice(1, 0, 'Feb');  // returns [], months is now ['Jan', 'Feb', 'March', 'April', 'June']

// At index 4, delete 1 element, insert 'May'
months.splice(4, 1, 'May');  // returns ['June'], months is now ['Jan', 'Feb', 'March', 'April', 'May']
```
### Difference between slice and splice?
- slice:
    - Non-mutating - returns a new array without modifying the original
    - Takes start and end indexes (end is exclusive)
    - Syntax: array.slice(startIndex, endIndex)
    - Used for copying portions of an array
- splice:
    - Mutating - modifies the original array
    - Takes start index, delete count, and items to insert
    - Returns an array of removed elements
    - Used for adding, removing, or replacing elements
```js
// slice example
const fruits = ['apple', 'banana', 'orange', 'mango'];
const citrus = fruits.slice(1, 3); // returns ['banana', 'orange']
// fruits remains unchanged: ['apple', 'banana', 'orange', 'mango']

// splice example
const vegetables = ['carrot', 'broccoli', 'cucumber', 'zucchini'];
const removed = vegetables.splice(1, 2, 'pepper', 'tomato');
// removed: ['broccoli', 'cucumber']
// vegetables is modified: ['carrot', 'pepper', 'tomato', 'zucchini']
```
### What does Array.isArray() do?
- Array.isArray(),Static method that checks if a value is an array
- Returns true if the value is an array, false otherwise
- More reliable than instanceof Array because it works across different execution contexts
```js
Array.isArray([1, 2, 3]);  // true
Array.isArray('hello');    // false
Array.isArray({key: 'value'}); // false
Array.isArray(null);       // false
```
### Difference between map and forEach?
- map:
    - Returns a new array with transformed elements
    - Original array remains unchanged
    - Chainable with other array methods
    - Used when you need to transform each element in an array
- forEach:
    - Returns undefined (doesn't return a value)
    - Used for executing a function for each element
    - Not chainable with other array methods
    - Used when you need side effects (like logging)
```js
const numbers = [1, 2, 3];
// map
const doubled = numbers.map(num => num * 2); 
// doubled: [2, 4, 6], numbers unchanged

// forEach
let sum = 0;
numbers.forEach(num => sum += num);
// sum: 6, numbers unchanged, no return value
```
### How does filter work?
- Creates a new array with all elements that pass the test function
- The callback should return true to keep the element, false to exclude it
- Original array remains unchanged
- Returns a new array that may be shorter than the original
```js
const numbers = [1, 2, 3, 4, 5, 6];
const evens = numbers.filter(num => num % 2 === 0);
// evens: [2, 4, 6], numbers unchanged
```
### How does reduce work?
- Executes a reducer function on each element, resulting in a single output value
- Takes a callback function with accumulator and current value parameters
- Optional initial value for the accumulator
- Processes the array from left to right
- Extremely versatile method that can implement many other array operations
```js
const numbers = [1, 2, 3, 4];
const sum = numbers.reduce((accumulator, currentValue) => accumulator + currentValue, 0);
// sum: 10

// Without initial value (starts with first element as accumulator)
const product = numbers.reduce((accumulator, currentValue) => accumulator * currentValue);
// product: 24 (1*2*3*4)
```
### What is findIndex()?
- Returns the index of the first element that satisfies the provided testing function
- Returns -1 if no element passes the test
- Stops execution once it finds a matching element
- Similar to indexOf() but uses a callback function for testing
```js
const people = [
  { name: 'John', age: 20 },
  { name: 'Jane', age: 30 },
  { name: 'Jim', age: 25 }
];

const index = people.findIndex(person => person.age > 25);
// index: 1 (Jane's position)
```
### How do you flatten an array?
- Using the flat() method: array.flat(depth)
- Creates a new array with sub-array elements concatenated recursively
- Default depth is 1
- Use Infinity for maximum depth
```js
const nested = [1, 2, [3, 4, [5, 6]]];
const flatSingleLevel = nested.flat();      // [1, 2, 3, 4, [5, 6]]
const flatTwoLevels = nested.flat(2);       // [1, 2, 3, 4, 5, 6]
const completelyFlat = nested.flat(Infinity); // [1, 2, 3, 4, 5, 6]

// Alternative for older browsers
function flattenArray(arr) {
  return [].concat(...arr);
}
```


