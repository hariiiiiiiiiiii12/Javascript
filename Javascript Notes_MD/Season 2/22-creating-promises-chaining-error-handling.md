# Episode 22: Creating a Promise, Chaining & Error Handling

In the previous topic, we learned how to **consume promises** using `.then()`.

In this topic we will learn:

- How to **create a Promise**
- How **Promise chaining** works
- How to handle **errors using `.catch()`**

---

# Consumer Part of a Promise

```javascript
const cart = ["shoes", "pants", "kurta"];

const promise = createOrder(cart);

promise.then(function (orderId) {
  proceedToPayment(orderId);
});
```

Here we assume that:

```
createOrder(cart)
```

returns a **Promise**.

Now the question is:

**How is this promise actually created?**

---

# Producer Part of a Promise

JavaScript provides a **Promise constructor** to create promises.

```javascript
function createOrder(cart) {

  const promise = new Promise(function (resolve, reject) {

    if (!validateCart(cart)) {
      const err = new Error("Cart is not Valid");
      reject(err);
    }

    const orderId = "12345";

    if (orderId) {
      resolve(orderId);
    }

  });

  return promise;
}
```

### Important Points

The Promise constructor receives a function with two parameters:

```
resolve
reject
```

These are functions provided by JavaScript.

- `resolve()` → used when the operation succeeds
- `reject()` → used when the operation fails

---

# Promise Pending State

Example:

```javascript
const cart = ["shoes", "pants", "kurta"];

const promise = createOrder(cart);

console.log(promise);
```

Output:

```
Promise { <pending> }
```

Why?

Because the asynchronous operation has not completed yet.

Once the promise resolves, the `.then()` callback executes.

---

# Consuming Promise

```javascript
promise.then(function (orderId) {
  proceedToPayment(orderId);
});
```

`.then()` runs only when the promise is **resolved**.

---

# Handling Errors using `.catch()`

If the promise gets rejected, we can handle the error using `.catch()`.

```javascript
const cart = ["shoes", "pants", "kurta"];

createOrder(cart)
  .then(function (orderId) {
    proceedToPayment(orderId);
  })
  .catch(function (err) {
    console.log(err);
  });
```

If `validateCart(cart)` returns false:

```
reject(err)
```

Then `.catch()` will execute.

---

# Promise Chaining

Promise chaining allows us to execute multiple asynchronous steps sequentially.

Example flow:

```
createOrder → proceedToPayment → showOrderSummary → updateWalletBalance
```

Example implementation:

```javascript
createOrder(cart)
  .then(function (orderId) {
    console.log(orderId);
    return orderId;
  })
  .then(function (orderId) {
    return proceedToPayment(orderId);
  })
  .then(function (paymentInfo) {
    console.log(paymentInfo);
  })
  .catch(function (err) {
    console.log(err);
  });
```

Important rule:

The value returned from one `.then()` becomes the input for the next `.then()`.

---

# Example Implementation

```javascript
function createOrder(cart) {

  return new Promise(function (resolve, reject) {

    if (!validateCart(cart)) {
      const err = new Error("Cart is not Valid");
      reject(err);
    }

    const orderId = "12345";

    if (orderId) {
      resolve(orderId);
    }

  });

}

function proceedToPayment(orderId) {

  return new Promise(function (resolve, reject) {

    resolve("Payment Successful");

  });

}
```

---

# What Happens When a Promise Fails

If any promise in the chain fails:

```
reject()
```

Execution jumps directly to `.catch()`.

Remaining `.then()` blocks are skipped.

---

# Continuing Execution After Catch

Sometimes we want execution to continue even after handling an error.

We can place `.catch()` in the middle of the chain.

Example:

```javascript
createOrder(cart)
  .then(function (orderId) {
    console.log(orderId);
    return orderId;
  })
  .catch(function (err) {
    console.log(err);
  })
  .then(function (orderId) {
    return proceedToPayment(orderId);
  })
  .then(function (paymentInfo) {
    console.log(paymentInfo);
  });
```

In this case:

- `.catch()` handles earlier errors
- execution continues with later `.then()` blocks

---

# Key Takeaways

- Promises are created using the **Promise constructor**
- The constructor receives `resolve` and `reject`
- `.then()` handles successful results
- `.catch()` handles errors
- Promise chaining allows sequential asynchronous operations
- The return value of one `.then()` becomes input for the next `.then()`
- If a promise fails, control moves to the nearest `.catch()`