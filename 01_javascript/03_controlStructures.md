### Conditional Statements

- used for decision making(if/ else if/ else).
- executes one block depending on the condition.
  ```js
  let age = 20;
  if (age < 13) {
    console.log("Child");
  } else if (age < 18) {
    console.log("Teen");
  } else {
    console.log("Adult");
  }
  // Output: "Adult"
  ```

### switch

- used when multiple conditions are checked against same variable.
- cleaner alternative for if-else
  ```js
  let day = 2;
  switch (day) {
    case 1:
      console.log("Monday");
      break;
    case 2:
      console.log("Tuesday");
      break;
    default:
      console.log("Other day");
  }
  // Output: "Tuesday"
  ```

### Looping Structures

- For loop
  - used for repeating tasks, if we know the no. of iterations.
    ```js
    for (let i = 0; i < 3; i++) {
      console.log(i);
    }
    // Output: 0 1 2
    ```
- while Loop
  - gets executed as long as the condition is true.
    ```js
    let i = 0;
    while (i < 3) {
      console.log(i);
      i++;
    }
    // Output: 0 1 2
    ```
- do-while
  - runs at least once, even if the condition is false.
    ```js
    let i = 0;
    do {
      console.log(i);
      i++;
    } while (i < 3);
    // Output: 0 1 2
    ```
- for-in
  - iterates over the object keys
  ```js
  let obj = { a: 1, b: 2 };
  for (let key in obj) {
    console.log(key, obj[key]);
  }
  // Output: a 1  b 2
  ```
- for-of
  - used to iterate over the values
    ```js
    let arr = [10, 20, 30];
    for (let value of arr) {
      console.log(value);
    }
    // Output: 10 20 30
    ```

### jumpStatements

- controls the flow inside the loops
  - break, exists the loop immediately
  - continue, skips the current iteration
    ```js
    for (let i = 0; i < 5; i++) {
      if (i === 2) continue; // skip 2
      if (i === 4) break; // stop at 4
      console.log(i);
    }
    // Output: 0 1 3
    ```

### Summary:

- Conditionals: if, else if, else, switch
- Loops: for, while, do...while, for...in, for...of
- Jump: break, continue
- if-else vs switch = ranges vs fixed values.
- for = initialization → condition → body → increment.
- while vs do...while = check before vs after.
- break exits loop, continue skips iteration, return exits function.
- switch supports strings; missing break → fall-through.
- Ternary can be nested but hurts readability.
- for...in → keys; for...of → values.

<hr><hr>

### Difference between if-else and switch?
- if-else: Best for range checks and complex conditions.
  - `if (x > 10) {...}          // good for ranges`
- switch: Best when comparing the same variable against multiple fixed values.
  - `switch(color) { case "red": ... }  // good for fixed matches`

### How does a for loop work?
- Structure: `for(initialization; condition; increment){}`
  - Initialization runs once.
  - Condition is checked → if true, execute body.
  - After each iteration, increment runs, then condition is rechecked.
    ```js
        for (let i = 0; i < 3; i++) {
        console.log(i);
        }
        // Output: 0 1 2
    ```
### What is the difference between while and do...while?
- while: Condition is checked before the loop body → may run zero times.
- do...while: Loop body executes once before checking condition → always runs at least once.
    ```js
        let i=5;
        while(i<5){ console.log("while"); }  // never runs
        do { console.log("do"); } while(i<5); // runs once
    ```

### Can you use `break` inside nested loops?
- ✅ Yes, `break` only exits the current innermost loop, not all loops

### Difference between `continue` and `break`?
- break: Exits loop entirely.
- continue: Skips current iteration, moves to next one.
    ```js
        for(let i=0;i<5;i++){
          if(i===2) continue; // skips 2
          if(i===4) break;    // exits at 4
          console.log(i);
        }
        // Output: 0 1 3
    ```

### What is the output of a `return` inside a loop?
- `return` immediately exits the entire function, not just the loop.
    ```js
    function test(){
      for(let i=0;i<5;i++){
        if(i===2) return i;
      }
    }
    console.log(test()); // Output: 2
    ```

### Can `switch` handle string cases?
- Yes. switch can compare numbers, strings, and even expressions.
    ```js
        let fruit = "apple";
        switch(fruit){
          case "apple": console.log("Fruit is apple"); break;
        }
        // Output: Fruit is apple
    ```
### What happens if you forget `break` in switch?
- Execution falls through into the next case(s) until a break is encountered.
    ```js
        let x=1;
        switch(x){
          case 1: console.log("One");
          case 2: console.log("Two");
        }
        // Output: "One" and "Two"
    ```

### Can you nest ternary operators?
- Yes, but it reduces readability.
    ```js
        let age=20;
        let msg = age<13 ? "Child" : age<18 ? "Teen" : "Adult";
        console.log(msg); // "Adult"
    ```
### Explain for...in vs for...of.
- for...in → Iterates over keys (property names) of an object.
- for...of → Iterates over values of an iterable (arrays, strings, etc).
    ```js
        let arr=[10,20,30];
        for (let i in arr) console.log(i);   // 0 1 2 (indexes)
        for (let v of arr) console.log(v);   // 10 20 30 (values)
    ```