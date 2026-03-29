# Episode 1 — Execution Context

## What is Execution Context?

Everything in JavaScript runs inside something called an **Execution Context**.

You can think of it like a **container where JavaScript code is executed**.  
Whenever a JavaScript program runs, the engine creates an execution context to manage the execution of that code.

It keeps track of:

- Variables
- Functions
- Scope
- The order in which code should run

---

## Components of an Execution Context

An execution context mainly has two parts:

### 1. Memory Component

The memory component stores all variables and functions as **key-value pairs**.

Example:

```javascript
var a = 10;

function greet() {
    console.log("Hello");
}
```

In memory this would look like:

```
a → 10
greet → function
```

During the initial phase, variables are first stored with the value **undefined**.

The memory component is also called the **Variable Environment**.

---

### 2. Code Component

The code component is where the **actual execution of the code happens**.

JavaScript executes the program **line by line** in this section.

Example:

```
Line 1 → executed
Line 2 → executed
Line 3 → executed
```

This part of the execution context is also called the **Thread of Execution**.

---

## How JavaScript Executes Code

JavaScript follows two important characteristics.

### JavaScript is Synchronous

This means code is executed **one step at a time in order**.

Example:

```javascript
console.log("First");
console.log("Second");
console.log("Third");
```

Output:

```
First
Second
Third
```

JavaScript will **not jump to the next line until the current line finishes execution**.

---

### JavaScript is Single Threaded

JavaScript can execute **only one command at a time**.

There is only **one call stack** and one main thread executing the code.

Example:

```
Task 1 → completed
Task 2 → completed
Task 3 → completed
```

Tasks do not run simultaneously on multiple threads in the main JavaScript engine.

---

## Summary

- JavaScript code runs inside an **Execution Context**.
- It acts like a **container that manages code execution**.
- It has two main components:
  - **Memory Component (Variable Environment)** — stores variables and functions.
  - **Code Component (Thread of Execution)** — executes code line by line.
- JavaScript is:
  - **Synchronous** — runs code in order.
  - **Single-threaded** — executes one task at a time.