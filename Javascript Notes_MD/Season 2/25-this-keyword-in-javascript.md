# Episode 25: `this` Keyword in JavaScript

In JavaScript, the `this` keyword refers to an object.

Which object `this` refers to **depends on how the function is invoked (called)**.

---

# `this` in Global Space

Anything defined globally is said to be in the **global space**.

```javascript
console.log(this);
```

In a browser environment, this prints:

```
window
```

So in browsers:

```
this === window
```

Note:

The global object depends on the runtime environment.

| Environment | Global Object |
|---|---|
| Browser | window |
| Node.js | global |

---

# `this` Inside a Function

```javascript
function x() {
  console.log(this);
}

x();
```

The value of `this` depends on **strict mode**.

### Non-Strict Mode

```
this → window object
```

### Strict Mode

```
this → undefined
```

Example:

```javascript
"use strict";

function x() {
  console.log(this);
}

x(); // undefined
```

---

# `this` Substitution

In **non-strict mode**, if `this` is:

```
null
or
undefined
```

JavaScript replaces it with the **global object**.

This behavior is called:

```
this substitution
```

Because of this:

```
this → window (in non-strict mode)
```

---

# How a Function is Called Matters

The value of `this` depends on **how the function is invoked**.

Example in strict mode:

```javascript
function x() {
  console.log(this);
}

x();        // undefined
window.x(); // window
```

---

# `this` Inside an Object Method

When a function is called as a **method of an object**, `this` refers to that object.

```javascript
const obj = {
  a: 10,
  x: function () {
    console.log(this);
    console.log(this.a);
  }
};

obj.x();
```

Output:

```
{ a: 10, x: f }
10
```

Here:

```
this → obj
```

---

# call(), apply() and bind()

These methods allow us to **explicitly control the value of `this`**.

Example:

```javascript
const student = {
  name: "Alok",
  printName: function () {
    console.log(this.name);
  }
};

student.printName();
```

Output:

```
Alok
```

Now another object:

```javascript
const student2 = {
  name: "Kajal"
};
```

To reuse the same function:

```javascript
student.printName.call(student2);
```

Output:

```
Kajal
```

Explanation:

```
call() sets the value of this
```

So inside `printName`, `this` now refers to `student2`.

The same applies to:

```
call()
apply()
bind()
```

All are used to **control the value of `this`**.

---

# `this` Inside Arrow Functions

Arrow functions **do not have their own `this`**.

Instead they take `this` from their **lexical surrounding context**.

Example:

```javascript
const obj = {
  a: 10,
  x: () => {
    console.log(this);
  }
};

obj.x();
```

Output:

```
window
```

Even though `x` is inside `obj`, arrow functions do not bind `this`.

They inherit it from the **outer scope**.

---

# Arrow Function Inside a Method

```javascript
const obj2 = {
  a: 10,
  x: function () {
    const y = () => {
      console.log(this);
    };
    y();
  }
};

obj2.x();
```

Output:

```
obj2
```

Explanation:

- `y` is an arrow function
- It takes `this` from its enclosing scope
- The enclosing scope is `x`
- `x` was called using `obj2.x()`

Therefore:

```
this → obj2
```

---

# `this` Inside DOM Elements

Inside DOM event handlers, `this` refers to the **HTML element that triggered the event**.

Example:

```html
<button onclick="alert(this)">
  Click Me
</button>
```

Output:

```
[object HTMLButtonElement]
```

So here:

```
this → button element
```

---

# Key Takeaways

- `this` refers to an object
- Its value depends on **how a function is called**
- In global scope, `this` refers to the **global object**
- In strict mode, `this` inside functions is **undefined**
- In object methods, `this` refers to the **object**
- `call`, `apply`, and `bind` allow manual control of `this`
- Arrow functions **inherit `this` from their lexical scope**
- In DOM events, `this` refers to the **HTML element**
```