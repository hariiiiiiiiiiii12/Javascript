# Episode 14: Callback Functions in JavaScript ft. Event Listeners

## Callback Functions

In JavaScript, functions are **first-class citizens**.  
This means functions can be:

- passed as arguments
- returned from other functions
- assigned to variables

When we pass a function as an argument to another function, that function is called a **callback function**.

A callback function gives another function the ability to **execute it later**.

Example:

```javascript
setTimeout(function () {
  console.log("Timer");
}, 1000);
```

Here:

- The first argument is a **callback function**
- The second argument is the **delay time in milliseconds**

The callback runs **after the timer finishes**.

---

## JavaScript and Asynchronous Behavior

JavaScript is:

- **Single-threaded**
- **Synchronous**

Meaning it executes code **one line at a time using a single call stack**.

However, using **callbacks**, JavaScript can perform **asynchronous operations**.

Example:

```javascript
setTimeout(function () {
  console.log("timer");
}, 5000);

function x(y) {
  console.log("x");
  y();
}

x(function y() {
  console.log("y");
});
```

Output:

```
x
y
timer
```

### Explanation

1. `x()` executes first
2. `y()` runs inside `x`
3. Both functions leave the **call stack**
4. After 5 seconds, the callback from `setTimeout` enters the call stack
5. `"timer"` is printed

---

## Call Stack and Blocking

All JavaScript functions execute through the **call stack**.

If any function takes too long to execute, it **blocks the main thread**.

Example:

If function `x()` takes **30 seconds**, JavaScript cannot execute anything else until it finishes.

This is called **blocking the main thread**.

Best practice:

Use **asynchronous operations** for long-running tasks.

---

## Example of Callback Usage

```javascript
function printStr(str, cb) {
  setTimeout(() => {
    console.log(str);
    cb();
  }, Math.floor(Math.random() * 100) + 1);
}

function printAll() {
  printStr("A", () => {
    printStr("B", () => {
      printStr("C", () => {});
    });
  });
}

printAll();
```

Output:

```
A
B
C
```

Even though delays are random, callbacks ensure the **correct order**.

---

# Event Listeners

Callbacks are also widely used with **event listeners**.

Example HTML:

```html
<button id="clickMe">Click Me!</button>
```

JavaScript:

```javascript
document
  .getElementById("clickMe")
  .addEventListener("click", function xyz() {
    console.log("Button clicked");
  });
```

Here:

- `"click"` is the event
- `xyz` is the **callback function**
- When the button is clicked, the callback function executes

---

# Increment Counter Example

### Using Global Variable (Not Recommended)

```javascript
let count = 0;

document
  .getElementById("clickMe")
  .addEventListener("click", function xyz() {
    console.log("Button clicked", ++count);
  });
```

Problem:

- Anyone can modify the `count` variable
- Global variables reduce code safety

---

# Using Closures (Better Approach)

```javascript
function attachEventList() {

  let count = 0;

  document
    .getElementById("clickMe")
    .addEventListener("click", function xyz() {
      console.log("Button clicked", ++count);
    });

}

attachEventList();
```

Here:

- `count` is **private**
- The callback function forms a **closure**
- Only the event listener can access `count`

This improves **data encapsulation**.

---

# Garbage Collection and Event Listeners

Event listeners can consume memory because they form **closures**.

Even when the call stack is empty, the event listener may still hold references to variables.

Example:

- `count` remains in memory
- Because the callback function might execute later

If many event listeners are added and not removed, they can cause **memory issues**.

---

# Best Practice

Remove event listeners when they are no longer needed.

Example:

- `removeEventListener()`

Too many listeners for events like:

- click
- scroll
- hover

can slow down a web page.

---

# Key Takeaways

- Callback functions are functions passed as arguments to other functions
- Callbacks enable **asynchronous programming in JavaScript**
- JavaScript uses a **single call stack**
- Long-running functions can **block the main thread**
- Event listeners rely heavily on callbacks
- Closures help maintain private state in event handlers
- Unnecessary event listeners can lead to **memory issues**