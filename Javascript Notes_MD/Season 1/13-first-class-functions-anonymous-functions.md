# Episode 13: First Class Functions & Anonymous Functions

> Functions are the heart of JavaScript.

Functions in JavaScript are extremely powerful because they behave like **values**.  
This allows them to be stored in variables, passed as arguments, and returned from other functions.

---

# Function Statement

A **Function Statement** is the most basic way of defining a function.

Example:

```javascript
function a() {
  console.log("Hello");
}

a(); // Hello
```

This type of function declaration is also called a **Function Declaration**.

---

# Function Expression

A **Function Expression** is when a function is assigned to a variable.

Example:

```javascript
var b = function () {
  console.log("Hello");
};

b();
```

Here:

- The function is treated like a **value**
- It is stored inside the variable `b`

---

# Difference Between Function Statement and Function Expression

The main difference between them is related to **hoisting**.

Example:

```javascript
a(); // "Hello A"
b(); // TypeError

function a() {
  console.log("Hello A");
}

var b = function () {
  console.log("Hello B");
};
```

### Why does this happen?

During the **memory creation phase**:

```
a -> function definition
b -> undefined
```

So:

- `a()` works because the function is already stored in memory.
- `b()` fails because `b` is initially `undefined`.

Only when execution reaches the line:

```
var b = function() {...}
```

does `b` get assigned a function.

---

# Function Declaration

A **Function Declaration** is simply another name for a **Function Statement**.

Example:

```javascript
function greet() {
  console.log("Hello");
}
```

---

# Anonymous Function

An **Anonymous Function** is a function without a name.

Example:

```javascript
function () {

}
```

This will produce a **SyntaxError**.

Why?

Because **function statements require a function name**.

Anonymous functions are usually used inside **function expressions**.

Example:

```javascript
var b = function () {
  console.log("Hello");
};
```

Here the function has **no name**, so it is an anonymous function.

---

# Named Function Expression

A **Named Function Expression** is a function expression where the function has its own name.

Example:

```javascript
var b = function xyz() {
  console.log("b called");
};

b();   // works
xyz(); // ReferenceError
```

Why?

The function name `xyz` is **not available in the global scope**.

It is only accessible **inside the function itself**.

---

# Parameters vs Arguments

Parameters are the variables defined in the function definition.

Arguments are the values passed when calling the function.

Example:

```javascript
var b = function (param1, param2) {
  console.log("b called");
};

b(arg1, arg2);
```

Here:

- `param1`, `param2` → Parameters
- `arg1`, `arg2` → Arguments

---

# First Class Functions (First Class Citizens)

JavaScript treats functions as **first class citizens**.

This means:

- Functions can be passed as arguments
- Functions can be returned from other functions
- Functions can be stored in variables

Example: Passing a function as an argument

```javascript
var b = function (param1) {
  console.log(param1);
};

b(function () {});
```

Output:

```
function () {}
```

---

# Passing a Named Function

Another way of passing a function as an argument:

```javascript
var b = function (param1) {
  console.log(param1);
};

function xyz() {}

b(xyz);
```

Here the function `xyz` is passed as an argument.

---

# Returning a Function

Functions can also return other functions.

Example:

```javascript
var b = function () {
  return function () {};
};

console.log(b());
```

Output:

```
function () {}
```

The function `b` returns another function.

---

# Key Takeaways

- Functions in JavaScript behave like **values**
- A **Function Statement** declares a function normally
- A **Function Expression** assigns a function to a variable
- **Anonymous Functions** do not have names
- **Named Function Expressions** have names but are not accessible globally
- Functions can be **passed as arguments**
- Functions can be **returned from other functions**
- This capability is known as **First Class Functions**