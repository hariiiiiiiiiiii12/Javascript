# Episode 6: undefined vs not defined in JavaScript

In JavaScript, beginners often get confused between **`undefined`** and **`not defined`**.  
Even though they sound similar, they mean **completely different things**.

To understand the difference, we need to recall how JavaScript executes code.

Whenever a JavaScript program runs, the **Execution Context** is created and it has two phases:

1. Memory Creation Phase
2. Code Execution Phase

---

# undefined

During the **Memory Creation Phase**, JavaScript allocates memory for variables.

At this stage, variables are given a special placeholder value called **`undefined`**.

This means:

- The variable **exists in memory**
- But **no value has been assigned yet**

Example:

```javascript
console.log(x); // undefined

var x = 25;

console.log(x); // 25
```

### What happens internally

During the memory phase:

```
x: undefined
```

During the execution phase:

```
console.log(x) -> undefined
x = 25
console.log(x) -> 25
```

So `undefined` simply means:

> The variable exists, but it currently has no assigned value.

---

# not defined

`not defined` occurs when JavaScript tries to access a variable that **was never declared in the program**.

Example:

```javascript
console.log(a);
```

Output:

```
Uncaught ReferenceError: a is not defined
```

This happens because:

- JavaScript never saw a declaration for `a`
- So no memory was allocated for it
- Therefore the engine cannot find it

---

# Key Difference

| Situation | Result |
|----------|--------|
| Variable declared but not assigned value | `undefined` |
| Variable never declared in the program | `not defined` |

Example:

```javascript
console.log(x); // undefined
var x = 25;

console.log(a); // ReferenceError: a is not defined
```

---

# JavaScript is Loosely Typed

JavaScript is a **loosely typed (or weakly typed) language**.

This means variables are **not permanently attached to a data type**.

The same variable can store different types of values at different times.

Example:

```javascript
var a = 5;

a = true;

a = "hello";
```

Here the variable `a` stores:

1. Number
2. Boolean
3. String

JavaScript allows this flexibility.

---

# Important Tip

Avoid manually assigning `undefined` to variables.

Example (not recommended):

```javascript
var x = undefined;
```

Why?

Because `undefined` already has a **special meaning in JavaScript** — it represents a variable that has been declared but not assigned a value yet.

Instead, if you want to intentionally indicate "no value", it is usually better to use:

```
null
```

---

# Key Takeaways

- `undefined` means **memory is allocated but no value is assigned**
- `not defined` means **the variable was never declared**
- `undefined` is automatically assigned during the memory creation phase
- JavaScript is a **loosely typed language**
- A variable can store different data types during execution
- Avoid manually assigning `undefined` to variables