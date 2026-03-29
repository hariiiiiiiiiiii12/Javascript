# Episode 20: Callback (Good Part and Bad Part)

Callbacks are a fundamental concept in JavaScript, especially when dealing with asynchronous operations.

There are two important aspects of callbacks:

1. Good Part of Callbacks
2. Bad Part of Callbacks

---

# Good Part of Callbacks

Callbacks are extremely useful when writing **asynchronous code** in JavaScript.

JavaScript is a **synchronous, single-threaded language**.

This means:

- It has only **one call stack**
- It executes **one task at a time**
- It does not wait for slow operations

Example:

```javascript
console.log("Namaste");
console.log("JavaScript");
console.log("Season 2");
```

Output:

```
Namaste
JavaScript
Season 2
```

JavaScript executes the code immediately because it does not wait.

---

# Delaying Execution Using Callback

If we want to delay execution, we can use a callback with `setTimeout`.

Example:

```javascript
console.log("Namaste");

setTimeout(function () {
  console.log("JavaScript");
}, 5000);

console.log("Season 2");
```

Output:

```
Namaste
Season 2
JavaScript
```

Explanation:

- `setTimeout` registers the callback
- JavaScript continues executing the remaining code
- After the timer finishes, the callback runs

This is one of the simplest examples of **asynchronous programming using callbacks**.

---

# E-Commerce Example

Consider an e-commerce application.

A user adds items to the cart and places an order.

Example cart:

```javascript
const cart = ["shoes", "pants", "kurta"];
```

The backend steps may look like this:

1. Create an order
2. Proceed to payment

Example:

```javascript
api.createOrder();
api.proceedToPayment();
```

However, there is a **dependency**.

Payment should happen **only after the order is created**.

Callbacks help handle such dependencies.

---

# Using Callback to Handle Dependency

```javascript
api.createOrder(cart, function () {
  api.proceedToPayment();
});
```

Explanation:

- `createOrder` is called first
- Once the order is created, it executes the callback
- The callback triggers `proceedToPayment`

---

# Adding More Steps

Suppose after payment we also want to show the order summary.

Now the flow becomes:

```javascript
api.createOrder(cart, function () {
  api.proceedToPayment(function () {
    api.showOrderSummary();
  });
});
```

---

# Even More Dependency

Now suppose we want to update the wallet after showing the summary.

```javascript
api.createOrder(cart, function () {
  api.proceedToPayment(function () {
    api.showOrderSummary(function () {
      api.updateWallet();
    });
  });
});
```

This creates deeply nested callbacks.

---

# Callback Hell

When multiple callbacks are nested inside each other, it creates a structure like this:

```
api.createOrder(cart, function () {
  api.proceedToPayment(function () {
    api.showOrderSummary(function () {
      api.updateWallet();
    });
  });
});
```

This problem is called **Callback Hell**.

Another name for this structure is:

```
Pyramid of Doom
```

Problems with callback hell:

- Hard to read
- Hard to maintain
- Hard to debug
- Difficult to scale

---

# Inversion of Control

Another major issue with callbacks is **Inversion of Control**.

Example:

```javascript
api.createOrder(cart, function () {
  api.proceedToPayment();
});
```

Here we are passing a callback function to `createOrder`.

This means we are **trusting `createOrder` to call our callback correctly**.

Possible risks:

- What if the callback is never called?
- What if it is called multiple times?
- What if the API is written by another developer and contains bugs?

In this situation we lose control over our code.

This problem is called **Inversion of Control**.

Definition:

> Inversion of Control happens when we pass a function as a callback and depend on another function to execute it.

---

# Why Async Programming Exists

Asynchronous programming in JavaScript exists because **callbacks exist**.

Callbacks allow JavaScript to perform operations like:

- API calls
- Timers
- File operations
- Event handling

without blocking the main thread.

---

# Key Problems with Callbacks

Callbacks introduce two major problems:

1. Callback Hell
2. Inversion of Control

These problems led to the introduction of **Promises**, which solve many of these issues.

Promises will be discussed in the next topic.