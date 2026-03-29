# Episode 8: let & const in JavaScript and the Temporal Dead Zone (TDZ)

In JavaScript, `let` and `const` were introduced in ES6 to improve variable handling compared to `var`.

Although `let` and `const` are **hoisted**, they behave differently from `var`.

Let's understand how.

---

# Example

```javascript
console.log(a); // ReferenceError: Cannot access 'a' before initialization
console.log(b); // undefined

let a = 10;

console.log(a); // 10

var b = 15;

console.log(window.a); // undefined
console.log(window.b); // 15
```

At first glance it may look like `let` variables are **not hoisted**, but they actually are.

The difference lies in **how they are stored and accessed before initialization**.

---

# What Happens During Hoisting

During the **memory creation phase**:

- `var` variables are stored inside the **global object**
- `let` and `const` variables are stored in a **separate memory space**

Example internal memory representation:

```
Global Memory
-------------
b: undefined

Script Memory
-------------
a: undefined
```

Because `b` belongs to the global object, it can be accessed before assignment (it prints `undefined`).

However `a` is stored in a **different memory space**, so JavaScript prevents access until it is initialized.

This is why the first `console.log(a)` throws an error.

---

# Temporal Dead Zone (TDZ)

The **Temporal Dead Zone** is the time between:

- when a `let` or `const` variable is hoisted
- and when it is actually initialized

Example:

```javascript
console.log(a); // TDZ error

let a = 10;
```

Everything between the **start of the scope** and `let a = 10` is the **Temporal Dead Zone**.

During this time the variable exists in memory but **cannot be accessed**.

Attempting to access it results in a **ReferenceError**.

---

# Accessing Variables Through the Global Object

`var` variables become properties of the global object (`window` in browsers).

Example:

```javascript
var b = 15;

console.log(window.b); // 15
console.log(this.b);   // 15
```

But `let` and `const` variables **do not attach to the global object**.

Example:

```javascript
let a = 10;

console.log(window.a); // undefined
console.log(this.a);   // undefined
```

---

# Syntax Errors with let

`let` does **not allow redeclaration in the same scope**.

Example:

```javascript
let a = 10;
let a = 100;
```

Output:

```
SyntaxError: Identifier 'a' has already been declared
```

Another invalid case:

```javascript
let a = 10;
var a = 100;
```

This also throws a **SyntaxError** because the same identifier cannot be declared again in the same scope.

---

# const in JavaScript

`const` is even stricter than `let`.

With `const`:

- the variable **must be initialized during declaration**
- reassignment is **not allowed**

Example:

```javascript
let a;
a = 10;

console.log(a); // 10
```

This works with `let`.

---

But this **does not work with const**:

```javascript
const b;
b = 10;
```

Output:

```
SyntaxError: Missing initializer in const declaration
```

Correct usage:

```javascript
const b = 100;
```

---

# Reassigning a const Variable

Example:

```javascript
const b = 100;

b = 1000;
```

Output:

```
TypeError: Assignment to constant variable
```

`const` variables cannot be reassigned after initialization.

---

# Types of Errors in JavaScript

## ReferenceError

Example:

```
Uncaught ReferenceError: x is not defined
```

Meaning:

The variable was **never declared in the program**.

---

Example:

```
Uncaught ReferenceError: Cannot access 'a' before initialization
```

Meaning:

The variable exists but is currently inside the **Temporal Dead Zone**.

---

## SyntaxError

Example:

```
Uncaught SyntaxError: Identifier 'a' has already been declared
```

Meaning:

JavaScript detected invalid syntax before running the code.

The program **does not execute at all**.

---

Example:

```
Uncaught SyntaxError: Missing initializer in const declaration
```

Meaning:

A `const` variable was declared without assigning a value.

---

## TypeError

Example:

```
Uncaught TypeError: Assignment to constant variable
```

Meaning:

A program tried to **change the value of a const variable**.

---

# Best Practices

- Prefer using **const** whenever possible
- Use **let** if the variable value needs to change
- Avoid using **var** in modern JavaScript
- Declare variables at the top of their scope to reduce the **Temporal Dead Zone**
- Always initialize variables when declaring them
```