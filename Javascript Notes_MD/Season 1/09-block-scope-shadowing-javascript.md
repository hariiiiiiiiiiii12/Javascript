# Episode 9: Block Scope & Shadowing in JavaScript

To understand modern JavaScript scoping, it is important to understand **Blocks**, **Block Scope**, and **Shadowing**.

---

# What is a Block?

A **Block** (also called a **compound statement**) is used to group multiple JavaScript statements together.

Blocks are created using curly braces `{ }`.

Example:

```javascript
{
    var a = 10;
    let b = 20;
    const c = 30;
}
```

Inside this block:

- `let` and `const` are **block scoped**
- `var` is **not block scoped**

Instead, `var` is **function scoped or globally scoped**.

---

# Block Scope Example

```javascript
{
    var a = 10;
    let b = 20;
    const c = 30;
}

console.log(a); // 10
console.log(b); // ReferenceError
console.log(c); // ReferenceError
```

### Why does this happen?

During hoisting:

- `let` and `const` variables are stored in a **separate memory space reserved for the block**
- `var` variables are stored in the **global scope**

Conceptually:

```
Global Scope
------------
a: 10

Block Scope
-----------
b: 20
c: 30
```

Because `b` and `c` belong to the block scope, they **cannot be accessed outside the block**.

---

# What is Shadowing?

**Shadowing** occurs when a variable declared inside a block **has the same name as a variable outside the block**.

The inner variable temporarily **shadows** the outer variable.

Example:

```javascript
var a = 100;

{
    var a = 10;
    let b = 20;
    const c = 30;

    console.log(a); // 10
    console.log(b); // 20
    console.log(c); // 30
}

console.log(a); // 10
```

### Why does this happen?

Since `var` is **not block scoped**, the `a` declared inside the block **actually modifies the global variable**.

So when `a = 10` executes, it replaces the global value `100`.

Memory representation:

```
Global Scope
------------
a: 10
```

There is **no separate block variable for `var a`**.

---

# Shadowing with let and const

Let's see the behavior with `let`.

```javascript
let b = 100;

{
    var a = 10;
    let b = 20;
    const c = 30;

    console.log(b); // 20
}

console.log(b); // 100
```

Here:

- The `b` inside the block is **different** from the `b` outside.
- Both exist in **separate memory spaces**.

Conceptually:

```
Global Scope
------------
b: 100

Block Scope
-----------
b: 20
c: 30
```

This works because `let` and `const` are **block scoped**.

---

# Shadowing in Functions

The same concept applies to functions.

Example:

```javascript
const c = 100;

function x() {
    const c = 10;
    console.log(c); // 10
}

x();

console.log(c); // 100
```

Here:

- The `c` inside the function shadows the global `c`.
- Both variables exist in different scopes.

---

# Illegal Shadowing

Some shadowing cases are **not allowed in JavaScript**.

Example:

```javascript
let a = 20;

{
    var a = 20;
}
```

This results in:

```
SyntaxError: Identifier 'a' has already been declared
```

### Why?

Because:

- `let` is block scoped
- `var` tries to declare the variable in the **same scope**

So JavaScript rejects the code.

---

# Valid Shadowing

Shadowing using `let` inside a block is valid.

Example:

```javascript
let a = 20;

{
    let a = 30;
    console.log(a); // 30
}

console.log(a); // 20
```

Both variables exist in **different scopes**.

---

# var Shadowed by let

This is valid:

```javascript
var a = 20;

{
    let a = 30;
}
```

Here:

- `var a` belongs to global scope
- `let a` belongs to block scope

So there is no conflict.

---

# Shadowing Inside Functions

Since `var` is **function scoped**, this is also valid:

```javascript
let a = 20;

function x() {
    var a = 30;
}
```

The `a` inside the function belongs to the **function scope**, not the global scope.

---

# Important Notes

- All the same scoping rules apply to **arrow functions**
- `var` is **function scoped**
- `let` and `const` are **block scoped**

---

# Key Takeaways

- A **Block** groups multiple JavaScript statements using `{ }`
- `let` and `const` are **block scoped**
- `var` is **function scoped**
- **Shadowing** occurs when an inner variable hides an outer variable with the same name
- Shadowing with `let` and `const` creates separate variables
- Using `var` inside a block can modify global variables
- Some combinations cause **Illegal Shadowing** and result in SyntaxError