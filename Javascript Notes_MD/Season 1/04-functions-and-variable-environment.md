# Episode 4: Functions and Variable Environments

In JavaScript, every function execution creates its own **Execution Context**.  
This means each function gets its **own variable environment**.

A **Variable Environment** is simply the place where variables and functions are stored within an execution context.

Let's understand this with an example.

---

# Example Code

```javascript
var x = 1;

a();
b(); // calling functions before defining them works because of hoisting

console.log(x); // 1

function a() {
  var x = 10; // local variable inside function a
  console.log(x);
}

function b() {
  var x = 100; // local variable inside function b
  console.log(x);
}
```

---

# Output

```
10
100
1
```

---

# Understanding the Behavior

Even though all variables are named `x`, they **do not affect each other**.

This happens because **each execution context has its own variable environment**.

So the variable `x` inside:

- Global scope
- Function `a`
- Function `b`

are all **different variables stored in different memory spaces**.

---

# Code Flow in Terms of Execution Context

## Step 1 — Global Execution Context Creation

When the program starts, JavaScript creates the **Global Execution Context (GEC)**.

The Global Execution Context is pushed into the **Call Stack**.

```
Call Stack
-----------
GEC
```

---

## Step 2 — Memory Creation Phase of GEC

JavaScript scans the code and allocates memory.

```
x: undefined

a: function a() { ... }

b: function b() { ... }
```

Functions store their **entire function definition in memory**, while variables are initialized with **undefined**.

---

## Step 3 — Code Execution Phase of GEC

Now JavaScript starts executing the code line by line.

### Line 1

```javascript
var x = 1;
```

Value of `x` in the global execution context becomes:

```
x = 1
```

---

## Step 4 — Function `a()` is Called

Whenever a function is invoked, JavaScript creates a **new execution context**.

This new context is pushed onto the **Call Stack**.

```
Call Stack
-----------
a()
GEC
```

---

# Execution Context for Function `a`

### Memory Phase

```
x: undefined
```

### Code Execution Phase

```javascript
var x = 10;
```

```
x = 10
```

Then:

```javascript
console.log(x);
```

Output:

```
10
```

After execution finishes, the execution context for `a()` is **removed from the Call Stack**.

```
Call Stack
-----------
GEC
```

---

# Step 5 — Function `b()` is Called

Again, a new execution context is created.

```
Call Stack
-----------
b()
GEC
```

---

# Execution Context for Function `b`

### Memory Phase

```
x: undefined
```

### Code Execution Phase

```javascript
var x = 100;
```

```
x = 100
```

Then:

```javascript
console.log(x);
```

Output:

```
100
```

After execution finishes, the execution context for `b()` is removed.

```
Call Stack
-----------
GEC
```

---

# Step 6 — Back to Global Execution Context

Now the next line executes:

```javascript
console.log(x);
```

Remember the global variable:

```
x = 1
```

Output:

```
1
```

---

# Program Completion

Once all lines of code finish executing:

- The **Global Execution Context is removed**
- The **Call Stack becomes empty**

```
Call Stack
-----------
(empty)
```

The JavaScript program has now finished executing.

---

# Key Takeaways

- Every function call creates a **new execution context**
- Each execution context has its **own variable environment**
- Variables inside functions **do not interfere with variables in other contexts**
- The **Call Stack** manages execution contexts
- JavaScript follows **Last In First Out (LIFO)** when managing execution contexts