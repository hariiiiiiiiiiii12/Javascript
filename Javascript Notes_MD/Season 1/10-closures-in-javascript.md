# Episode 10: Closures in JavaScript

A **closure** is created when a function is bundled together with its **lexical environment**.

In simple terms:

> A closure is a function that remembers variables from its outer scope even after that outer function has finished executing.

JavaScript supports **lexical scoping**, meaning a function can access variables defined in its outer scope.

---

# Basic Example

```javascript
function x() {
    var a = 7;

    function y() {
        console.log(a);
    }

    return y;
}

var z = x();

console.log(z); // prints the function y
```

Explanation:

- Function `x()` defines a variable `a`
- Inside `x()`, another function `y()` is defined
- `y()` accesses variable `a`

When `x()` finishes executing, it returns `y`.

But something interesting happens:

`y` still **remembers the variable `a`** even after `x()` has finished.

This happens because of **closures**.

When `y` is returned, it carries with it:

- the function itself
- the **lexical environment of `x()`**

So when `z()` is called later, it can still access `a`.

---

# Understanding What Gets Returned

When we execute:

```javascript
var z = x();
```

The returned value stored in `z` is:

```
function y() {
    console.log(a);
}
```

But internally it is actually:

```
Closure = function y + reference to variable a
```

So the function remembers its **outer environment**.

---

# Example with Multiple Closures

```javascript
function z() {
    var b = 900;

    function x() {
        var a = 7;

        function y() {
            console.log(a, b);
        }

        y();
    }

    x();
}

z(); // 7 900
```

Explanation:

Here the function `y()` has access to:

- variable `a` from function `x`
- variable `b` from function `z`

JavaScript looks for variables using the **scope chain**.

Search order:

```
y() -> x() -> z() -> Global Scope
```

---

# Closure Definition

A commonly used definition of closure:

> A closure is a function that has access to variables from its outer scope even after the outer function has returned.

This means the function **remembers the environment where it was created**.

---

# Advantages of Closures

Closures are widely used in JavaScript for several important patterns.

---

# 1. Module Design Pattern

Closures help create **private variables and functions**.

Example:

```javascript
const authModule = (function () {

    let loggedInUser = null;

    function login(username, password) {
        loggedInUser = username;
    }

    function logout() {
        loggedInUser = null;
    }

    function getUserInfo() {
        return loggedInUser;
    }

    return {
        login,
        logout,
        getUserInfo
    };

})();

authModule.login("john_doe", "secret");

console.log(authModule.getUserInfo()); // john_doe
```

Here the variable `loggedInUser` is **private**.

It cannot be accessed directly from outside the module.

---

# 2. Currying

Closures help create **functions that remember arguments**.

Example:

```javascript
const calculateTotalPrice = (taxRate) => (price) => {
    return price + price * (taxRate / 100);
};

const calculateSalesTax = calculateTotalPrice(8);

console.log(calculateSalesTax(100)); // 108
```

Here the inner function remembers the `taxRate`.

---

# 3. Memoization

Closures help store results of expensive function calls.

Example:

```javascript
function fibonacci(n, memo = {}) {

    if (n in memo) return memo[n];

    if (n <= 1) return n;

    memo[n] = fibonacci(n - 1, memo) + fibonacci(n - 2, memo);

    return memo[n];
}

console.log(fibonacci(10)); // 55
```

Memoization improves performance by **caching previous results**.

---

# 4. Data Hiding and Encapsulation

Closures allow creating **private data**.

Example:

```javascript
class Person {

    #name;

    constructor(name) {
        this.#name = name;
    }

    getName() {
        return this.#name;
    }
}

const person = new Person("Alice");

console.log(person.getName()); // Alice
```

Private fields prevent direct access to internal data.

---

# 5. setTimeout and Asynchronous Behavior

Closures are also used in asynchronous operations.

Example:

```javascript
function showMessage(message, delay) {

    setTimeout(() => {
        console.log(message);
    }, delay);

}

showMessage("Hello World", 2000);
```

The callback function remembers the variable `message` using closures.

---

# Disadvantages of Closures

Although closures are powerful, they also have some drawbacks.

- They can **consume more memory**
- If used incorrectly they may cause **memory leaks**
- Excessive closures may **slow down performance**

So closures should be used **carefully in large applications**.

---

# Key Takeaways

- A closure is a **function bundled with its lexical environment**
- Inner functions can access variables from outer functions
- Closures allow functions to **remember variables even after outer functions return**
- Closures are used in:
  - module pattern
  - currying
  - memoization
  - data encapsulation
  - asynchronous callbacks