# Episode 17: Trust Issues with setTimeout()

The `setTimeout()` function does not always guarantee that the callback will execute exactly after the specified delay.

For example, if we use a timer of 5 seconds, the callback might execute after:

- 5 seconds
- 6 seconds
- 10 seconds

The actual execution time depends on the **Call Stack and Event Loop**.

---

# Example

```javascript
console.log("Start");

setTimeout(function cb() {
  console.log("Callback");
}, 5000);

console.log("End");

// Assume millions of lines of code run after this
```

Expected output:

```
Start
End
Callback
```

However, the callback may run later than 5 seconds depending on the state of the call stack.

---

# What Actually Happens Internally

Step-by-step execution:

1. Global Execution Context (GEC) is created and pushed into the call stack.
2. `"Start"` is printed in the console.
3. `setTimeout()` registers the callback function in the **Web API environment** and starts a timer of 5 seconds.
4. JavaScript does not wait for the timer and continues execution.
5. `"End"` is printed in the console.

Now assume there are **millions of lines of code** after this that take about **10 seconds** to execute.

While the call stack is executing those lines:

- The timer in the Web API environment completes after 5 seconds.
- The callback function is moved to the **Callback Queue**.

However, the callback cannot execute immediately.

Why?

Because the **Call Stack is still busy executing code**.

---

# Role of the Event Loop

The **Event Loop** continuously checks:

1. Is the Call Stack empty?
2. Is there anything in the Callback Queue?

If the call stack is not empty, the event loop **cannot move the callback into the call stack**.

So even though the timer finished after 5 seconds, the callback might execute after 10 seconds or more.

Once the call stack becomes empty:

```
Callback Queue → Event Loop → Call Stack
```

The callback is pushed to the call stack and executed immediately.

---

# Concurrency Model of JavaScript

This behavior is part of JavaScript's **concurrency model**.

JavaScript is:

- Single-threaded
- Uses a single call stack
- Uses the Event Loop to manage asynchronous operations

Because of this, asynchronous callbacks must wait until the **main thread is free**.

---

# Rule of JavaScript

The first rule of JavaScript performance:

Do not **block the main thread**.

Since JavaScript has only one call stack, blocking the main thread prevents other tasks from executing.

---

# setTimeout Guarantees Minimum Delay

`setTimeout()` guarantees **at least** the specified delay, not an exact delay.

For example:

```
setTimeout(callback, 5000)
```

means:

```
callback will run after at least 5000ms
```

but possibly later if the call stack is busy.

---

# What Happens When Timeout is 0?

Example:

```javascript
console.log("Start");

setTimeout(function cb() {
  console.log("Callback");
}, 0);

console.log("End");
```

Output:

```
Start
End
Callback
```

Even though the timer is `0ms`, the callback still goes through:

1. Web API environment
2. Callback Queue
3. Event Loop
4. Call Stack

So the callback executes **only after the call stack becomes empty**.

---

# Why Use setTimeout(..., 0)?

Using `setTimeout(fn, 0)` is sometimes used to **defer execution**.

It allows the current synchronous code to finish first before executing less important tasks.

Example use case:

Allow important operations to complete before running background tasks.

---

# Key Takeaways

- `setTimeout()` does not guarantee exact timing.
- It guarantees only a **minimum delay**.
- Callbacks must wait until the **call stack becomes empty**.
- The **Event Loop** moves callbacks from the queue to the call stack.
- Blocking the main thread delays asynchronous callbacks.
- Even `setTimeout(fn, 0)` executes only after the call stack clears.
```