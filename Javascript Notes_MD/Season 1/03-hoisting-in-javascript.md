# Episode 3: Hoisting in JavaScript (Variables & Functions)

One of the most commonly discussed behaviors in JavaScript is **Hoisting**.

To understand hoisting properly, we need to remember something from the previous topic:

JavaScript executes code inside an **Execution Context**, and every execution context has two phases:

1. Memory Creation Phase
2. Code Execution Phase

During the **memory creation phase**, JavaScript allocates memory for variables and functions **before executing any code**.

This behavior is what leads to **hoisting**.

---

# Example 1

```javascript
getName(); // Namaste Javascript
console.log(x); // undefined

var x = 7;

function getName() {
  console.log("Namaste Javascript");
}
```

At first glance, this code might seem incorrect.

In many programming languages, calling a function **before it is defined** would immediately throw an error.

But JavaScript behaves differently.

---

# What Actually Happens Internally

Before executing the code, JavaScript performs the **Memory Creation Phase**.

During this phase:

- Memory is allocated for variables
- Memory is allocated for functions

The memory state looks like this:

```
x: undefined

getName: function getName() {
  console.log("Namaste Javascript");
}
```

Notice the difference:

- Variables are initialized with **undefined**
- Functions store their **entire function definition**

---

# Code Execution Phase

Now JavaScript starts executing the code **line by line**.

### Step 1

```javascript
getName();
```

The function already exists in memory, so it executes successfully.

Output:

```
Namaste Javascript
```

---

### Step 2

```javascript
console.log(x);
```

The variable `x` exists in memory but has not been assigned a value yet.

So the output is:

```
undefined
```

---

### Step 3

```javascript
var x = 7;
```

Now the value `7` is assigned to `x`.

---

# Important Observation

If we completely remove the variable declaration:

```javascript
console.log(x);
```

Then JavaScript throws an error:

```
Uncaught ReferenceError: x is not defined
```

This happens because **no memory was allocated for `x` at all**.

---

# What is Hoisting?

**Hoisting** is the JavaScript behavior where **variable and function declarations are moved to the top of their scope during the memory creation phase**.

Because of hoisting:

- Variables can be accessed before assignment (they will be `undefined`)
- Functions can be called before their declaration

This is not magic — it happens because JavaScript **prepares memory before executing code**.

---

# Example 2

```javascript
getName(); // Namaste JavaScript
console.log(x); // Uncaught ReferenceError: x is not defined
console.log(getName);

function getName() {
  console.log("Namaste JavaScript");
}
```

Let's see what happens.

During the **memory creation phase**:

```
getName: function getName() {
  console.log("Namaste JavaScript");
}
```

There is **no variable `x` declared anywhere**, so it does not exist in memory.

---

### Execution

1️⃣ `getName()` runs successfully

Output:

```
Namaste JavaScript
```

2️⃣ `console.log(x)`

This throws an error because `x` was never declared.

```
Uncaught ReferenceError: x is not defined
```

3️⃣ `console.log(getName)`

This would print the **entire function definition**, because functions are stored in memory during the memory phase.

---

# Example 3 (Function Expression)

Now let's look at a different scenario.

```javascript
getName(); // Uncaught TypeError: getName is not a function
console.log(getName);

var getName = function () {
  console.log("Namaste JavaScript");
};
```

This produces a **TypeError**.

---

# What Happens Internally

During the **memory creation phase**:

```
getName: undefined
```

Why?

Because `getName` is declared using **var**, so it behaves like a normal variable.

The function is **not stored yet**.

---

# Code Execution

### Step 1

```javascript
getName();
```

At this moment:

```
getName = undefined
```

JavaScript tries to call `undefined` as a function.

That results in:

```
Uncaught TypeError: getName is not a function
```

The program stops here, so the rest of the code does not execute.

---

# Key Takeaways

- Hoisting happens because of the **memory creation phase of the execution context**
- Variable declarations are hoisted and initialized with **undefined**
- Function declarations are hoisted with their **entire function definition**
- Function expressions behave like **normal variables**
- Calling a function expression before assignment results in a **TypeError**
- Accessing a completely undeclared variable results in a **ReferenceError**