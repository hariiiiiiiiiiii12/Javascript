# Episode 15: Asynchronous JavaScript & Event Loop (from Scratch)

> Time, tide and JavaScript wait for none.

The **Call Stack** executes any execution context that enters it.  
However, the call stack itself **does not have any concept of timers**.

To understand asynchronous JavaScript, we must understand how JavaScript interacts with **Web APIs, Callback Queue, Microtask Queue, and the Event Loop**.

---

# JavaScript Runtime Environment

The browser environment contains several components:

- JavaScript Engine
- Call Stack
- Web APIs
- Callback Queue
- Microtask Queue
- Event Loop

Inside the JavaScript engine, we mainly have:

```
Call Stack
```

The call stack executes:

- Global Execution Context
- Function Execution Contexts

---

# Browser Superpowers

Browsers provide several additional features beyond the JavaScript engine.

Examples include:

- Local Storage
- Timers
- DOM APIs
- Geolocation
- Network requests
- Console logging

JavaScript interacts with these capabilities using **Web APIs**.

---

# Web APIs

Web APIs are **not part of JavaScript itself**.

They are features provided by the **browser environment**.

Examples include:

- `setTimeout()`
- DOM APIs
- `fetch()`
- `localStorage`
- `console`
- `location`

Example usage:

```javascript
window.setTimeout()
window.console.log()
window.localStorage
```

Since `window` is the global object in browsers, we usually omit it.

Example:

```javascript
setTimeout(...)
console.log(...)
```

---

# Example: setTimeout

```javascript
console.log("Start");

setTimeout(function cb() {
  console.log("Timer");
}, 5000);

console.log("End");
```

Output:

```
Start
End
Timer
```

---

# Step-by-Step Execution

1. Global Execution Context is created and pushed into the call stack.
2. `"Start"` is printed using the console Web API.
3. `setTimeout` registers a callback function and starts a timer.
4. The callback is stored in the **Web API environment**.
5. `"End"` is printed.
6. Global Execution Context is removed from the call stack.

Meanwhile:

- The timer continues running in the background.
- When the timer reaches `0`, the callback is ready to execute.

However, the callback **cannot go directly to the call stack**.

---

# Callback Queue

When the timer expires:

```
cb() → Callback Queue
```

The callback queue stores tasks waiting to be executed.

But the callback still needs permission to enter the call stack.

---

# Event Loop

The **Event Loop** constantly checks:

1. Is the call stack empty?
2. Is the callback queue non-empty?

If both conditions are satisfied, the event loop moves the callback into the call stack.

Example flow:

```
Callback Queue → Event Loop → Call Stack
```

Then the callback function executes.

---

# Event Listener Example

```javascript
console.log("Start");

document
  .getElementById("btn")
  .addEventListener("click", function cb() {
    console.log("Callback");
  });

console.log("End");
```

Output:

```
Start
End
```

Later, when the user clicks the button:

```
Callback
```

Explanation:

- The event listener registers a callback in the **Web API environment**
- The callback remains there until the event occurs
- When the button is clicked, the callback moves to the **Callback Queue**
- The event loop eventually pushes it into the call stack

---

# Why Do We Need a Callback Queue?

Imagine the user clicks a button **6 times**.

This results in:

```
Callback Queue
--------------
cb()
cb()
cb()
cb()
cb()
cb()
```

The event loop processes them **one by one** and sends them to the call stack.

---

# Fetch and Microtask Queue

Now consider the following example.

```javascript
console.log("Start");

setTimeout(function cbT() {
  console.log("CB Timeout");
}, 5000);

fetch("https://api.netflix.com")
  .then(function cbF() {
    console.log("CB Netflix");
  });

console.log("End");
```

Assume:

- `fetch()` returns data after **2 seconds**
- `setTimeout()` timer is **5 seconds**

Output:

```
Start
End
CB Netflix
CB Timeout
```

---

# Why Does Fetch Execute First?

Because **Promises use the Microtask Queue**.

There are two queues:

```
Microtask Queue
Callback Queue
```

Priority:

```
Microtask Queue > Callback Queue
```

So the execution order becomes:

1. Execute global code
2. Run microtasks
3. Run callbacks

---

# What Goes Into Microtask Queue?

The following callbacks go into the **Microtask Queue**:

- Promise `.then()` callbacks
- Promise `.catch()` callbacks
- Promise `.finally()` callbacks
- `MutationObserver`

Example:

```javascript
fetch(...).then(...)
```

---

# What Goes Into Callback Queue?

Everything else goes into the **Callback Queue** (also called the Task Queue).

Examples:

- `setTimeout`
- `setInterval`
- DOM event callbacks
- Event listeners

---

# Microtask Queue Starvation

If the microtask queue keeps generating new tasks continuously, the callback queue may **never get executed**.

This situation is called:

```
Starvation
```

Because lower priority tasks never get CPU time.

---

# Important Questions

### When does the event loop start?

The event loop runs continuously as long as the JavaScript runtime is active.

It is almost an **infinite loop**.

---

### Do synchronous callbacks use Web APIs?

No.

Functions passed to synchronous methods like:

```
map()
filter()
reduce()
```

do **not** go through the Web API environment.

Only **asynchronous callbacks** use Web APIs.

---

### What does the Web API store?

Web APIs store the **callback function and its reference**.

For event listeners, the callback stays registered until:

- the event listener is removed
- or the browser session ends

---

### Does `setTimeout(fn, 0)` run immediately?

No.

Even if the delay is `0`, the callback must wait until the **call stack is empty**.

So it might execute after several milliseconds depending on the workload.

---

# Key Takeaways

- JavaScript is **single-threaded**
- The **Call Stack** executes code
- **Web APIs** provide browser features
- **Callback Queue** stores asynchronous tasks
- **Microtask Queue** has higher priority than the callback queue
- The **Event Loop** moves tasks from queues to the call stack
- Promises use the **Microtask Queue**
- `setTimeout` and events use the **Callback Queue**
```