# Episode 21: Promises in JavaScript

Promises are used to handle asynchronous operations in JavaScript.

To understand promises properly, it helps to first look at how asynchronous code was handled **before promises** using callbacks.

---

# Problem with Callback Approach

Consider an example of an e-commerce application.

```javascript
const cart = ["shoes", "pants", "kurta"];

const orderId = createOrder(cart);
proceedToPayment(orderId);
```

Both `createOrder` and `proceedToPayment` are asynchronous operations and have a dependency.

Payment should happen **only after the order is created**.

Using callbacks, this might look like:

```javascript
createOrder(cart, function () {
  proceedToPayment(orderId);
});
```

In this approach:

- We pass a callback function
- We rely on `createOrder` to call it

This introduces a major issue called **Inversion of Control**.

---

# Fixing the Problem Using Promises

Instead of passing callbacks, the `createOrder` function can return a **Promise**.

```javascript
const promiseRef = createOrder(cart);
```

The variable `promiseRef` now holds a **Promise object**.

A promise can be thought of as a placeholder for a value that will be available in the future.

Initially:

```
Promise { <pending> }
```

When the asynchronous operation finishes, the promise gets resolved with the actual data.

---

# Using `.then()` with Promises

To access the result when it becomes available, we attach a callback using `.then()`.

```javascript
promiseRef.then(function () {
  proceedToPayment(orderId);
});
```

Key difference:

- Earlier we **passed a function** to another function
- Now we **attach a function** to a promise

A promise guarantees that:

- the callback will be called when the data is ready
- it will be executed **only once**

---

# Real Example Using fetch()

The `fetch()` API returns a promise.

Example:

```javascript
const URL = "https://api.github.com/users/alok722";

const user = fetch(URL);

console.log(user);
```

Output:

```
Promise { <pending> }
```

The promise object internally contains:

- prototype
- promiseState
- promiseResult

Initially:

```
promiseState = pending
promiseResult = undefined
```

When the API response arrives:

```
promiseState = fulfilled
promiseResult = response data
```

---

# Accessing Promise Data

We can access the resolved data using `.then()`.

```javascript
const URL = "https://api.github.com/users/alok722";

const user = fetch(URL);

user.then(function (data) {
  console.log(data);
});
```

A promise can be in three states:

| State | Meaning |
|------|--------|
| pending | Initial state |
| fulfilled | Operation completed successfully |
| rejected | Operation failed |

---

# Promise Objects are Immutable

Once a promise is fulfilled or rejected, its result **cannot be changed**.

The promise object itself cannot be mutated directly.

We must access the result using `.then()`.

---

# Solving Callback Hell with Promises

Callback version:

```javascript
createOrder(cart, function (orderId) {
  proceedToPayment(orderId, function (paymentInfo) {
    showOrderSummary(paymentInfo, function (balance) {
      updateWalletBalance(balance);
    });
  });
});
```

This creates deeply nested code called **Callback Hell**.

---

# Promise Chaining

Promises solve this problem using **chaining**.

```javascript
createOrder(cart)
  .then(function (orderId) {
    return proceedToPayment(orderId);
  })
  .then(function (paymentInfo) {
    return showOrderSummary(paymentInfo);
  })
  .then(function (balance) {
    return updateWalletBalance(balance);
  });
```

Each `.then()` returns a promise.

The value returned from one `.then()` becomes the input for the next `.then()`.

---

# Common Pitfall in Promise Chaining

A common mistake is forgetting to **return the promise**.

Incorrect:

```javascript
createOrder(cart)
  .then(function (orderId) {
    proceedToPayment(orderId);
  });
```

Correct:

```javascript
createOrder(cart)
  .then(function (orderId) {
    return proceedToPayment(orderId);
  });
```

Returning the promise ensures the chain continues properly.

---

# Arrow Function Version

For better readability:

```javascript
createOrder(cart)
  .then((orderId) => proceedToPayment(orderId))
  .then((paymentInfo) => showOrderSummary(paymentInfo))
  .then((balance) => updateWalletBalance(balance));
```

---

# Interview Definitions

### What is a Promise?

A promise is:

- a placeholder for a future value
- a container for the result of an asynchronous operation
- an object representing the eventual completion or failure of an asynchronous operation

---

# Key Takeaways

- Promises help manage asynchronous operations
- They solve **Inversion of Control**
- They avoid **Callback Hell**
- `.then()` attaches callbacks to promises
- Promises support chaining
- Promise states: pending, fulfilled, rejected