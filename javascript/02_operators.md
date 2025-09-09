## Operators
### Arithmetic Operators
- used for mathematical addition
- +, -, *, /, %, **, ++, --
### Assignment Operator
- used to assign values to the variables
- =, +=, -=, *=, /=, %=, **=
### Comparision Operator
- returns a boolean value, i.e either true or false.
- ==, ===, !=, !==, >, <, >=, <=
### Logical Operator
- used with the booleans
- &&, ||, !
### Bitwise Operator
- works on 32 bit integers at bit level
- &, |, ^, ~, <<, >>, >>>
### String Operator
- Only `+`, `+=` applied to the strings
    ```js
        let first = "Hello";
        let second = " World";
        console.log(first + second); // "Hello World"
    ```
### Ternary(Conditional) Operator
- shorthand for if-else
- returns "yes" if condition is true, otherwise "no"
    ```js
        let age = 18;
        let canVote = (age >= 18) ? "Yes" : "No";
        console.log(canVote); // "Yes"
    ```
### TypeOperators
- `typeof`
  - `typeof "Hello"`    // "string"
- `instanceof`
  - `[1,2,3] instanceof Array`   //true
### Nullish Coalescing & Optional Chaining
- `??`, returns right hand operand if left hand is `null` or `undefined`
    ```js
        let user = null;
        console.log(user ?? "Guest"); // "Guest"
    ```
- `?.`, safely access the nested properties
    ```js
        let person = {};
        console.log(person.address?.city); // undefined
    ```

<hr><hr>

### Difference between `==` and `===`?
  - `==`, Loose equality. which compares the values after type conversion
  - `===`, strict Equality. which compares both the value and types having no conversion.
    ```js
        5 == "5"    //true
        5 === "5"   //false
    ```
### What is the difference between `++i` and `i++`?
  - `++1`, preIncrement => Increments first, then returns the new value.
  - `1++`, postIncrement => returns the current value, then increments.
    ```js
        let i = 5;
        console.log(++i);   //6, Increments then returns the value
        console.log(i++);   //6, returns, then increments
    ```
### Explain the difference between `&&` and `||`.
  - `&&`, returns first falsy value or last value if all are truthy.
  - `||`, returns first truthy value or last if none are truthy.
    ```js
        true && "Hello"  // "Hello" (all truthy → last one)
        false && "Hi"    // false (first falsy)
        true || "Hello"  // true (first truthy)
        false || "Hi"    // "Hi" (second is truthy)
    ```
### What does the nullish coalescing operator `??` do?
  - returns right hand operand only if leftHand side is `null` or `undefined`.
  - unlike ||, it does not treat 0 or "" as falsy.
    ```js
        let user = null;
        console.log(user ?? "Guest"); // "Guest"

        let count = 0;
        console.log(count ?? 10); // 0 (not null/undefined)
    ```
### What does `!!value` return?
  - converts any value to the equivalent boolean
  - First `!`, neglates truthy/falsy values
  - Second `!`, negates again providing the actual boolean.
    ```js
        !!"Hello"   //true, nonEmpty string is truthy
        !!0,    //false, 0 is falsy
        !!null,     //false
    ```
### What is the result of `2 + '2'`? Why
  - "22", `+`operator does string concatenation if either any of the operand is string.
  - Number 2 is converted to "2" → "2" + "2" = "22".
### What is the result of `'5' - 2`? Why
  - 3, `-`operator only works with the numbers
  - Here, "5" is coerced into number 5. => `5 - 2 = 3`
### Explain operator precedence in JavaScript.
  - Determines using the order of evaluation. where higher precedence operators are evaluated first.
  - Parentheses () → Highest precedence.
  - Unary (++, --, !, typeof, delete) → Next.
  - Arithmetic (*, /, %) before (+, -).
  - Comparison (<, >, <=, >=) before equality (==, ===).
  - Logical AND && before OR ||.
  - Assignment (=, +=, -=) → Lowest precedence.
