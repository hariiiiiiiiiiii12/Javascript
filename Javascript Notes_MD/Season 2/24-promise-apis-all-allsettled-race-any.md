# Episode 24: Promise APIs (all, allSettled, race, any)

## Overview

JavaScript provides several built-in Promise APIs to handle multiple asynchronous operations efficiently.

The four commonly used Promise APIs are:

- Promise.all()
- Promise.allSettled()
- Promise.race()
- Promise.any()

Understanding these APIs is important when working with asynchronous JavaScript.

---

# Promise.all()

`Promise.all()` is used when we want to execute multiple promises in parallel and wait for **all of them to complete successfully**.

Syntax:

```javascript
Promise.all([p1, p2, p3])
```

Example:

Assume:

- `p1` resolves in **3 seconds**
- `p2` resolves in **1 second**
- `p3` resolves in **2 seconds**

`Promise.all()` will wait **3 seconds** and then return:

```
["value1", "value2", "value3"]
```

Important behavior:

- It waits for **all promises to resolve**
- If **any promise rejects**, it immediately rejects

This behavior is called **Fail Fast**.

Example:

```javascript
const p1 = new Promise((resolve) => {
  setTimeout(() => resolve("P1 Success"), 3000);
});

const p2 = new Promise((resolve) => {
  setTimeout(() => resolve("P2 Success"), 1000);
});

const p3 = new Promise((resolve) => {
  setTimeout(() => resolve("P3 Success"), 2000);
});

Promise.all([p1, p2, p3]).then((results) => {
  console.log(results);
});
```

Output after 3 seconds:

```
["P1 Success", "P2 Success", "P3 Success"]
```

If any promise fails:

```javascript
const p2 = new Promise((resolve, reject) => {
  setTimeout(() => reject("P2 Fail"), 1000);
});

Promise.all([p1, p2, p3])
  .then((results) => console.log(results))
  .catch((err) => console.error(err));
```

Output after 1 second:

```
P2 Fail
```

---

# Promise.allSettled()

`Promise.allSettled()` waits for **all promises to finish**, regardless of success or failure.

Syntax:

```javascript
Promise.allSettled([p1, p2, p3])
```

Unlike `Promise.all`, it does not fail fast.

Example:

```javascript
const p1 = new Promise((resolve) => {
  setTimeout(() => resolve("P1 Success"), 3000);
});

const p2 = new Promise((resolve) => {
  setTimeout(() => resolve("P2 Success"), 1000);
});

const p3 = new Promise((resolve, reject) => {
  setTimeout(() => reject("P3 Fail"), 2000);
});

Promise.allSettled([p1, p2, p3]).then((results) => {
  console.log(results);
});
```

Output:

```
[
 { status: "fulfilled", value: "P1 Success" },
 { status: "fulfilled", value: "P2 Success" },
 { status: "rejected", reason: "P3 Fail" }
]
```

Key difference:

```
Promise.all() → Fail Fast
Promise.allSettled() → Waits for all results
```

---

# Promise.race()

`Promise.race()` returns the result of the **first promise that settles**.

"Settles" means either **resolved or rejected**.

Syntax:

```javascript
Promise.race([p1, p2, p3])
```

Example:

```javascript
const p1 = new Promise((resolve) => {
  setTimeout(() => resolve("P1 Success"), 3000);
});

const p2 = new Promise((resolve) => {
  setTimeout(() => resolve("P2 Success"), 1000);
});

const p3 = new Promise((resolve, reject) => {
  setTimeout(() => reject("P3 Fail"), 2000);
});

Promise.race([p1, p2, p3])
  .then((result) => console.log(result))
  .catch((err) => console.error(err));
```

Output after 1 second:

```
P2 Success
```

If the first settled promise rejects:

```
P3 Fail
```

---

# Promise.any()

`Promise.any()` returns the **first successfully fulfilled promise**.

Syntax:

```javascript
Promise.any([p1, p2, p3])
```

Important behavior:

- It ignores rejected promises
- It resolves when the **first promise succeeds**

Example:

```javascript
const p1 = new Promise((resolve) => {
  setTimeout(() => resolve("P1 Success"), 3000);
});

const p2 = new Promise((resolve) => {
  setTimeout(() => resolve("P2 Success"), 5000);
});

const p3 = new Promise((resolve, reject) => {
  setTimeout(() => reject("P3 Fail"), 2000);
});

Promise.any([p1, p2, p3])
  .then((result) => console.log(result))
  .catch((err) => console.error(err));
```

Output after 3 seconds:

```
P1 Success
```

---

# When All Promises Fail

If every promise rejects, `Promise.any()` returns an **AggregateError**.

Example:

```javascript
const p1 = Promise.reject("P1 Fail");
const p2 = Promise.reject("P2 Fail");
const p3 = Promise.reject("P3 Fail");

Promise.any([p1, p2, p3])
  .then((result) => console.log(result))
  .catch((err) => {
    console.error(err);
    console.error(err.errors);
  });
```

Output:

```
AggregateError: All promises were rejected
["P1 Fail", "P2 Fail", "P3 Fail"]
```

---

# Promise States

When a promise finishes execution, it becomes **settled**.

Settled states:

1. fulfilled (resolved)
2. rejected

---

# Additional Promise APIs

JavaScript also provides two additional helper methods.

### Promise.resolve()

Creates a resolved promise.

```javascript
Promise.resolve("Success");
```

---

### Promise.reject()

Creates a rejected promise.

```javascript
Promise.reject("Error");
```

---

# Summary

| API | Behavior |
|----|----|
| Promise.all | Waits for all promises, fails immediately if any reject |
| Promise.allSettled | Waits for all promises regardless of success or failure |
| Promise.race | Returns the first settled promise |
| Promise.any | Returns the first fulfilled promise |
| Promise.resolve | Creates a resolved promise |
| Promise.reject | Creates a rejected promise |

In practice, **Promise.all()** is the most commonly used Promise API.