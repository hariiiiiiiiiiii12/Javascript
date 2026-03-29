# Episode 23: async / await

## Topics Covered

- What is `async`
- What is `await`
- How async/await works behind the scenes
- Examples of async/await
- Error handling
- async/await vs Promise.then/.catch

---

# What is async?

`async` is a keyword used before a function to create an **asynchronous function**.

Example:

```javascript
async function getData() {
  return "Namaste JavaScript";
}

const dataPromise = getData();

console.log(dataPromise);
```

Output:

```
Promise { <fulfilled>: "Namaste JavaScript" }
```

Important rule:

An **async function always returns a Promise**, even if we return a normal value.

The returned value is automatically wrapped inside a Promise.

---

# Extracting Value from Async Function

```javascript
dataPromise.then((res) => console.log(res));
```

Output:

```
Namaste JavaScript
```

---

# Async Function Returning a Promise

Example:

```javascript
const p = new Promise((resolve, reject) => {
  resolve("Promise resolved value!!");
});

async function getData() {
  return p;
}

const dataPromise = getData();

console.log(dataPromise);

dataPromise.then((res) => console.log(res));
```

In this case:

- Since we already return a Promise
- `async` simply returns that Promise directly

---

# What is await?

`await` is used to handle Promises inside an async function.

Rule:

```
await can only be used inside an async function
```

Example:

```javascript
const p = new Promise((resolve, reject) => {
  resolve("Promise resolved value!!");
});

async function handlePromise() {
  const val = await p;
  console.log(val);
}

handlePromise();
```

Output:

```
Promise resolved value!!
```

---

# Promise.then vs async/await

Example Promise:

```javascript
const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Promise resolved value!!");
  }, 3000);
});
```

---

## Using `.then()`

```javascript
function getData() {
  p.then((res) => console.log(res));
  console.log("Hello There!");
}

getData();
```

Output:

```
Hello There!
Promise resolved value!!
```

Explanation:

- JavaScript does not wait
- It continues executing the next lines

---

## Using async/await

```javascript
async function handlePromise() {
  const val = await p;
  console.log("Hello There!");
  console.log(val);
}

handlePromise();
```

Output (after 3 seconds):

```
Hello There!
Promise resolved value!!
```

The code appears to wait at `await`.

---

# Multiple await Example

```javascript
async function handlePromise() {
  console.log("Hi");

  const val = await p;
  console.log("Hello There!");
  console.log(val);

  const val2 = await p;
  console.log("Hello There! 2");
  console.log(val2);
}

handlePromise();
```

Execution:

1. `Hi` prints immediately
2. Program waits for promise resolution
3. Remaining logs execute

---

# Example with Two Promises

```javascript
const p = new Promise((resolve) => {
  setTimeout(() => {
    resolve("Promise resolved value!!");
  }, 3000);
});

const p2 = new Promise((resolve) => {
  setTimeout(() => {
    resolve("Promise resolved value by p2!!");
  }, 2000);
});
```

### Case 1

```javascript
async function handlePromise() {
  console.log("Hi");

  const val = await p;
  console.log(val);

  const val2 = await p2;
  console.log(val2);
}

handlePromise();
```

Output:

```
Hi
(after 3s)
Promise resolved value!!
Promise resolved value by p2!!
```

Even though `p2` resolves earlier, execution waits for `p`.

---

### Case 2 (Reversed)

```javascript
async function handlePromise() {
  console.log("Hi");

  const val = await p2;
  console.log(val);

  const val2 = await p;
  console.log(val2);
}

handlePromise();
```

Output:

```
Hi
(after 2s)
Promise resolved value by p2!!
(after 1 more second)
Promise resolved value!!
```

---

# What Happens Behind the Scenes?

Important concept:

JavaScript **does not block the call stack**.

When `await` is encountered:

1. Execution of the async function is **paused**
2. The function is removed from the call stack
3. When the Promise resolves, execution **resumes from the same point**

This allows JavaScript to remain **non-blocking**.

---

# Real World Example

Example using `fetch` API:

```javascript
async function handlePromise() {

  const data = await fetch("https://api.github.com/users/alok722");

  const res = await data.json();

  console.log(res);

}

handlePromise();
```

Flow:

1. `fetch()` returns a Promise
2. `await` waits for the response
3. `.json()` also returns a Promise
4. The final result is printed

---

# Error Handling

In Promise syntax we use `.catch()`.

In async/await we use **try-catch**.

Example:

```javascript
async function handlePromise() {
  try {
    const data = await fetch("https://api.github.com/users/alok722");
    const res = await data.json();
    console.log(res);
  } catch (err) {
    console.log(err);
  }
}

handlePromise();
```

---

# Alternative Error Handling

Since async functions return promises:

```javascript
handlePromise().catch((err) => console.log(err));
```

---

# async/await vs Promise.then()

Important fact:

```
async/await is syntactic sugar over promises
```

This means:

- Internally it still uses Promises
- It simply provides a cleaner syntax

Benefits of async/await:

- Better readability
- Easier error handling
- Avoids long promise chains
- Code looks synchronous

---

# Key Takeaways

- `async` functions always return a Promise
- `await` pauses execution until a Promise resolves
- `await` can only be used inside async functions
- JavaScript does not block the call stack
- Async/await improves readability of asynchronous code
- Error handling can be done using try-catch
```