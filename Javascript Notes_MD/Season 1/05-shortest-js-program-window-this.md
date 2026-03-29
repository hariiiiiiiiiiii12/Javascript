# Episode 5: Shortest JS Program, window & this Keyword

The **shortest JavaScript program** is actually an **empty file**.

Even if the file contains no code, the JavaScript engine still performs several operations internally before running the program.

Whenever a JavaScript program runs, the engine automatically creates a **Global Execution Context (GEC)**.

The Global Execution Context contains two main components:

1. Memory Component (Variable Environment)
2. Code Component (Thread of Execution)

So even an empty file results in a **Global Execution Context being created**.

---

# Global Object and `this`

Along with the Global Execution Context, JavaScript also creates:

- A **Global Object**
- A **this keyword**

In browsers, the global object is called **`window`**.

The **window object** contains many built-in functions and variables that JavaScript provides by default.

Examples include:

- `setTimeout()`
- `setInterval()`
- `console`
- `alert()`
- `document`

These functions are available globally because they exist inside the **window object**.

---

# Relationship Between `this` and `window`

At the **global level in browsers**, the `this` keyword points to the global object.

So in the browser environment:

```
this === window
```

Both refer to the same object.

---

# Global Object in Different Environments

The name of the global object depends on the JavaScript runtime environment.

Examples:

| Environment | Global Object |
|-------------|---------------|
| Browser | window |
| Node.js | global |
| Modern JS environments | globalThis |

So while browsers use **window**, Node.js uses **global**.

---

# Global Variables and the Window Object

When a variable is declared in the **global scope using `var`**, it automatically becomes a property of the **global object**.

Example:

```javascript
var x = 10;

console.log(x);        // 10
console.log(this.x);   // 10
console.log(window.x); // 10
```

Explanation:

- `x` is created in the global scope
- Because of this, it gets attached to the **window object**
- Therefore we can access it using:
  - `x`
  - `this.x`
  - `window.x`

All three refer to the same variable.

---

# Why This Happens

When the Global Execution Context is created:

1. JavaScript creates the **Global Object**
2. JavaScript creates the **this keyword**
3. Global variables declared with `var` get attached to the **global object**

This is why global variables can be accessed through the `window` object in browsers.

---

# Key Takeaways

- The shortest JavaScript program is an **empty file**
- Even an empty program creates a **Global Execution Context**
- JavaScript automatically creates a **Global Object**
- In browsers, the global object is **window**
- At the global level: `this === window`
- Global variables declared with `var` become **properties of the window object**
- Different environments have different global objects (window, global, globalThis)