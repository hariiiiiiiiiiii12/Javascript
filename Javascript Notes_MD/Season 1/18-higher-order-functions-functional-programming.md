# Episode 18: Higher-Order Functions ft. Functional Programming

## What is a Higher-Order Function?

A **Higher-Order Function (HOF)** is a function that:

- takes one or more functions as arguments, or
- returns a function as a result.

Example:

```javascript
function x() {
    console.log("Hi");
}

function y(x) {
    x();
}

y(x); // Hi
```

Explanation:

- `y` is a **Higher-Order Function** because it receives a function as an argument.
- `x` is a **Callback Function** because it is passed to another function.

---

# Interview Example: Calculating Area of Circles

Suppose we have an array of circle radii and we want to calculate their areas.

### First Approach

```javascript
const radius = [1, 2, 3, 4];

const calculateArea = function(radius) {
    const output = [];

    for (let i = 0; i < radius.length; i++) {
        output.push(Math.PI * radius[i] * radius[i]);
    }

    return output;
}

console.log(calculateArea(radius));
```

This solution works correctly.

However, suppose we now need to calculate the **circumference** of circles.

---

# Second Requirement: Circumference

```javascript
const radius = [1, 2, 3, 4];

const calculateCircumference = function(radius) {
    const output = [];

    for (let i = 0; i < radius.length; i++) {
        output.push(2 * Math.PI * radius[i]);
    }

    return output;
}

console.log(calculateCircumference(radius));
```

Problem with this approach:

- We are repeating the same looping logic.
- This violates the **DRY Principle (Don't Repeat Yourself)**.

---

# Better Approach Using Higher-Order Functions

Instead of repeating the loop logic, we can separate the **operation logic** from the **iteration logic**.

```javascript
const radiusArr = [1, 2, 3, 4];

const area = function(radius) {
    return Math.PI * radius * radius;
}

const circumference = function(radius) {
    return 2 * Math.PI * radius;
}

const calculate = function(radiusArr, operation) {
    const output = [];

    for (let i = 0; i < radiusArr.length; i++) {
        output.push(operation(radiusArr[i]));
    }

    return output;
}

console.log(calculate(radiusArr, area));
console.log(calculate(radiusArr, circumference));
```

Explanation:

- `calculate` is a **Higher-Order Function**.
- `area` and `circumference` are **callback functions**.
- The logic for the operation is separated from the iteration logic.

This approach improves **code reusability and maintainability**.

---

# Connection with Functional Programming

This pattern demonstrates the concept of **Functional Programming**:

- Functions are treated as values.
- Logic is separated into reusable functions.
- Code becomes modular and easier to maintain.

---

# Polyfill of map()

The `calculate` function is conceptually similar to JavaScript's built-in `map()` function.

Example:

```javascript
radiusArr.map(area)
```

is similar to:

```javascript
calculate(radiusArr, area)
```

---

# Implementing map() Manually (Polyfill)

We can create our own version of `map()`.

```javascript
Array.prototype.calculate = function(operation) {

    const output = [];

    for (let i = 0; i < this.length; i++) {
        output.push(operation(this[i]));
    }

    return output;
}

console.log(radiusArr.calculate(area));
```

Explanation:

- `Array.prototype` allows us to add custom methods to all arrays.
- `this` refers to the array on which the function is called.

Example:

```javascript
radiusArr.calculate(area)
```

works similarly to:

```javascript
radiusArr.map(area)
```

---

# Key Takeaways

- A **Higher-Order Function** takes functions as arguments or returns functions.
- Callback functions are passed to higher-order functions.
- Higher-order functions improve **code reusability**.
- They support **functional programming patterns**.
- Built-in methods like `map`, `filter`, and `reduce` are examples of higher-order functions.