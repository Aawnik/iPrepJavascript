## TypeConversion
- Type conversion (or type casting) is the process of converting data from one type to another in JavaScript. It's a fundamental concept to understand because JavaScript is a loosely typed language.

### Implicit vs Explicit Type Conversion
- Implicit Conversion(Coercion)
    - This happens automatically when JavaScript converts one data type to another without explicit instruction:
    ```js
        let num = 10;
        let str = "The number is: " + num;  // num is implicitly    converted to string
        console.log(str);  // "The number is: 10"

        console.log("5" - 2);  // 3 (string "5" is converted to number)
        console.log("5" + 2);  // "52" (number 2 is converted to string)
    ```
- Explicit Conversion
    - This happens when you deliberately convert a value using conversion functions:
    ```js
        let str = "123";
        let num = Number(str);  // Explicitly convert string to number
        console.log(num);  // 123

        let bool = Boolean(1);  // Explicitly convert to boolean
        console.log(bool);  // true
    ```
### string to Number Conversion
```js
// Using Number() function
console.log(Number("123"));      // 123
console.log(Number("123.45"));   // 123.45
console.log(Number("123abc"));   // NaN (Not a Number)
console.log(Number(""));         // 0
console.log(Number(null));       // 0
console.log(Number(undefined));  // NaN

// Using parseInt() - for integers only
console.log(parseInt("123"));       // 123
console.log(parseInt("123.45"));    // 123 (decimal part is truncated)
console.log(parseInt("123abc"));    // 123 (stops at non-numeric character)
console.log(parseInt("abc123"));    // NaN (no leading numeric character)

// Using parseFloat() - for floating point numbers
console.log(parseFloat("123.45"));  // 123.45
console.log(parseFloat("123"));     // 123
```
### number to String Conversion
```js
// Using String() function
console.log(String(123));      // "123"
console.log(String(123.45));   // "123.45"
console.log(String(NaN));      // "NaN"
console.log(String(Infinity)); // "Infinity"

// Using toString() method
console.log((123).toString());      // "123"
console.log((123.45).toString());   // "123.45"

// With radix (base)
console.log((16).toString(16));     // "10" (hexadecimal)
console.log((2).toString(2));       // "10" (binary)

// String concatenation (implicit conversion)
console.log(123 + "");              // "123"
```
### toBoolean Conversion
```js
// Using Boolean() function
console.log(Boolean(1));        // true
console.log(Boolean(0));        // false
console.log(Boolean("hello"));  // true
console.log(Boolean(""));       // false
console.log(Boolean(null));     // false
console.log(Boolean(undefined));// false
console.log(Boolean(NaN));      // false
console.log(Boolean([]));       // true (empty array is truthy)
console.log(Boolean({}));       // true (empty object is truthy)

// Double negation (!!)
console.log(!!1);               // true
console.log(!!"hello");         // true
console.log(!!0);               // false
console.log(!!"");              // false
```
### falsy values in javascript
- The following values are always falsy
    - false
    - 0 (zero)
    - "" (empty string)
    - null
    - undefined
    - NaN

### Type Conversion rules and Quirks
```js
// Equality operator (==) performs type conversion
console.log(1 == "1");           // true
console.log(0 == "");            // true
console.log(0 == false);         // true
console.log("" == false);        // true

// Strict equality (===) doesn't perform type conversion
console.log(1 === "1");          // false
console.log(0 === "");           // false
console.log(0 === false);        // false
console.log("" === false);       // false

// Addition vs other operators
console.log("5" + 2);            // "52" (string concatenation)
console.log("5" - 2);            // 3 (numeric operation)
console.log("5" * 2);            // 10 (numeric operation)
console.log("5" / 2);            // 2.5 (numeric operation)
```
### Object to PrimitiveConversion
```js
// toString and valueOf methods
const obj = {
  toString() { return "Custom String"; },
  valueOf() { return 42; }
};

console.log(String(obj));     // "Custom String" (calls toString)
console.log(Number(obj));     // 42 (calls valueOf)
console.log(obj + "");        // "42" (calls valueOf first, then converts to string)
console.log(obj + 0);         // 42 (calls valueOf)

// Array conversion
console.log(String([1, 2, 3]));   // "1,2,3"
console.log([1, 2, 3] + "");      // "1,2,3"
console.log(Number([1]));         // 1
console.log(Number([1, 2]));      // NaN
```
### Array-like to Array Conversion
```js
// Convert array-like objects to arrays
function example() {
  // arguments is an array-like object
  const args = Array.from(arguments);
  // Now args is a real array
  console.log(args.map(x => x * 2));
}
example(1, 2, 3);  // [2, 4, 6]

// Alternative using spread operator
function example2() {
  const args = [...arguments];
  console.log(args);
}
example2(1, 2, 3);  // [1, 2, 3]
```
### Best Practice
- Use explicit conversion when possible for clarity
- Prefer === (strict equality) over == to avoid unexpected type conversion
- Be aware of falsy values in conditional statements
- Use Number(), parseInt(), or parseFloat() based on your specific needs
- Always validate user inputs before conversion to handle potential errors

<hr><hr>

### Difference between implicit and explicit type conversion?
- Implicit type conversion (coercion):
    - Happens automatically when JavaScript converts one data type to another without explicit instruction
    - Performed by the JavaScript engine during operations between different data types
    - Less predictable and can lead to unexpected results
```js
    let num = 5;
    let str = "10";
    console.log(num + str);  // "510" (number implicitly converted to string)
    console.log(str - num);  // 5 (string implicitly converted to number)
```
- Explicit type conversion:
    - Deliberately performed by the developer using conversion functions
    - More predictable and clearer for code maintenance
    - Uses functions like Number(), String(), Boolean(), etc.
```js
    let str = "42";
    let num = Number(str);  // Explicitly convert string to number
```

### How do you convert a string to a number?
- methods to convert strings to numbers:
```js
    // Using Number() function
    let num1 = Number("123");  // 123

    // Using parseInt() function
    let num2 = parseInt("123");  // 123

    // Using parseFloat() function
    let num3 = parseFloat("123.45");  // 123.45

    // Using unary plus operator
    let num4 = +"123";  // 123

    // Using multiplication (*1)
    let num5 = "123" * 1;  // 123
```

### Difference between parseInt and Number()?
- parseInt():
    - Parses a string and returns an integer
    - Takes a second parameter for radix (number base)
    - Stops parsing at the first non-numeric character
    - Returns NaN only if the first character isn't numeric
```js
    parseInt("123.45");    // 123 (ignores decimal part)
    parseInt("123abc");    // 123 (stops at 'a')
    parseInt("abc123");    // NaN (first character not numeric)
    parseInt("10", 2);     // 2 (parses "10" as binary)
```
- Number():
    - Converts the entire string to a number (integer or float)
    - Returns NaN if ANY character is non-numeric
    - Doesn't accept a radix parameter
    - Handles empty strings, null, and whitespace specially
```js
    Number("123.45");      // 123.45 (preserves decimal)
    Number("123abc");      // NaN (entire string must be numeric)
    Number("");            // 0
    Number("  ");          // 0
    Number(null);          // 0
```
- NOTE:_Use `parseInt()` when you need to extract a numeric value from the start of a string or when working with different number bases. Use `Number()` when you need to validate that an entire string represents a valid number.

### How do you convert a number to a string?
```js
    // Using String() function
    let str1 = String(123);  // "123"

    // Using toString() method
    let str2 = (123).toString();  // "123"

    // With radix for different number bases
    let str3 = (16).toString(16);  // "10" (hexadecimal)

    // Using string concatenation
    let str4 = 123 + "";  // "123"

    // Using template literals
    let str5 = `${123}`;  // "123"
```
### What does Boolean("false") return?
- Boolean("false") returns true.
- because "false" is a non-empty string, and all non-empty strings in JavaScript are truthy values when converted to boolean. Only the empty string ("") would return false. The string "false" is not the same as the boolean value false.

### What does Number("123abc") return?
- Number("123abc") returns NaN (Not a Number).
- Number() tries to convert the entire string to a number and requires the complete string to be valid numeric content. When it encounters non-numeric characters ('a', 'b', 'c'), the conversion fails. This differs from parseInt(), which would return 123 because it stops at the first non-numeric character.

### How do you check if a value is NaN?
- Number.isNaN() is more reliable because it only returns true for the actual NaN value, while the global isNaN() function returns true for anything that would convert to NaN, which can lead to false positives.
```js
    // Using isNaN() (global function)
    console.log(isNaN(NaN));          // true
    console.log(isNaN("hello"));      // true (converts to NaN first)

    // Using Number.isNaN() (more precise)
    console.log(Number.isNaN(NaN));    // true
    console.log(Number.isNaN("hello")); // false (doesn't convert)

    // Using self-comparison (NaN is the only value not equal to itself)
    const x = NaN;
    console.log(x !== x);             // true
```
### Difference between == type coercion vs === strict equality?
- == (loose equality):
    - Performs type coercion if operands are of different types
    - Converts operands to the same type before comparison
    - Can lead to unexpected results
```js
    console.log(1 == "1");       // true (string converted to number)
    console.log(0 == false);     // true (boolean converted to number)
    console.log(null == undefined); // true (both considered equal)
```
- === (strict equality):
    - Does not perform type coercion
    - Returns true only if operands are of the same type AND have the same value
    - Generally recommended for predictable comparisons
```js
    console.log(1 === "1");       // false (different types)
    console.log(0 === false);     // false (different types)
    console.log(null === undefined); // false (different types)
```
- REASON:_Using === is generally safer as it avoids unexpected type coercion and makes code behavior more predictable. It clearly requires both value and type to match.

### How do you convert array-like objects to arrays?
- Array-like objects (like arguments, NodeList) have numeric indices and length but don't have array methods.
```js
    // Using Array.from()
    function example1() {
      const args = Array.from(arguments);
      console.log(args.map(x => x * 2)); // Now we can use map
    }

    // Using spread operator
    function example2() {
      const args = [...arguments];
      console.log(args);
    }

    // Using Array.prototype.slice
    function example3() {
      const args = Array.prototype.slice.call(arguments);
      console.log(args);
    }

    example1(1, 2, 3);  // [2, 4, 6]
```
- Converting array-like objects to true arrays allows you to use array methods like map, filter, and reduce, which are not available on array-like objects directly.

### What is toString() used for?
- toString() is a method that converts a value to its string representation.
```js
    // For numbers
    console.log((123).toString());     // "123"
    console.log((123.45).toString());  // "123.45"

    // With radix for different bases
    console.log((16).toString(16));    // "10" (hexadecimal)
    console.log((8).toString(2));      // "1000" (binary)

    // For arrays
    console.log([1, 2, 3].toString()); // "1,2,3"

    // For custom objects (default)
    console.log({}.toString());        // "[object Object]"

    // For custom objects (overridden)
    const person = {
      name: "John",
      toString() {
        return `Person: ${this.name}`;
      }
    };
    console.log(person.toString());    // "Person: John"
```
- `toString()` provides a way to get string representations of values, especially useful for numbers with different bases (binary, hex, etc.) and for custom string representations of objects. It's also implicitly called in string contexts when objects need to be represented as strings.




