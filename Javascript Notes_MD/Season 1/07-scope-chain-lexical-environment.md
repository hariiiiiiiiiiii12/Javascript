# Episode 7: Scope Chain, Scope & Lexical Environment

To understand how JavaScript finds variables, we need to understand three related concepts:

- Scope
- Lexical Environment
- Scope Chain

These concepts explain **how JavaScript resolves variables during execution**.

---

# What is Scope?

**Scope** determines where a variable can be accessed in a program.

In JavaScript, variables can exist in different scopes such as:

- Global scope
- Function scope
- Block scope (with `let` and `const`)

Scope in JavaScript is closely related to something called the **Lexical Environment**.

---

# Example Cases

Let's examine a few examples and understand what happens.

---

# Case 1

```javascript
function a() {
    console.log(b);
}

var b = 10;

a();
```

Output:

```
10
```

### Why?

The function `a()` does not have a variable named `b` inside it.

So JavaScript looks **outside the function** and finds `b` in the **global scope**.

Therefore the output is `10`.

---

# Case 2

```javascript
function a() {
    c();

    function c() {
        console.log(b);
    }
}

var b = 10;

a();
```

Output:

```
10
```

### Why?

Here the function `c()` is nested inside `a()`.

The variable `b` is not present in `c()` or `a()`.

So JavaScript again searches **in the global scope** and finds `b = 10`.

---

# Case 3

```javascript
function a() {
    c();

    function c() {
        var b = 100;
        console.log(b);
    }
}

var b = 10;

a();
```

Output:

```
100
```

### Why?

Inside function `c()`, a new variable `b` is declared.

This **local variable takes precedence** over the global variable.

So JavaScript uses the nearest variable available in the scope.

---

# Case 4

```javascript
function a() {
    var b = 10;

    c();

    function c() {
        console.log(b);
    }
}

a();

console.log(b);
```

Output:

```
10
Uncaught ReferenceError: b is not defined
```

### Why?

Function `c()` can access variable `b` because it is defined inside its **outer function `a()`**.

However, the global scope **cannot access variables defined inside functions**.

So `console.log(b)` outside the function throws an error.

---

# Understanding with Execution Context

During execution, the Call Stack may look like this:

```
Call Stack
-----------
GEC
a()
c()
```

Now each execution context has its own **Lexical Environment**.

```
c()  -> lexical environment pointer -> a()

a()  -> b:10
        c:{function}
        lexical environment pointer -> GEC

GEC -> a:{function}
       lexical environment pointer -> null
```

This structure allows JavaScript to search variables in outer environments.

---

# What is a Lexical Environment?

A **Lexical Environment** is made up of:

```
Lexical Environment = Local Memory + Reference to Parent Lexical Environment
```

So each execution context contains:

- Its **local variables**
- A **reference to its parent's environment**

This reference allows JavaScript to look outside the current scope.

---

# Meaning of "Lexical"

The word **lexical** simply means:

> Based on the physical structure or location of the code.

JavaScript determines scope **based on where functions are written in the code**, not where they are called.

This is called **Lexical Scope** or **Static Scope**.

---

# Example of Lexical Structure

```javascript
function outer() {

    function inner() {
        // inner can access outer variables
    }

}
```

Conceptually it looks like this:

```
Global
 └── Outer
      └── Inner
```

The `inner` function is **lexically inside** `outer`, so it can access variables from `outer`.

---

# What is the Scope Chain?

When JavaScript tries to find a variable, it searches in this order:

1. Current function scope
2. Parent function scope
3. Global scope

This step-by-step search process is called the **Scope Chain**.

If the variable is not found anywhere in the chain, JavaScript throws:

```
ReferenceError
```

---

# Example of Scope Chain

```javascript
function a() {

    function c() {
        console.log(x);
    }

    c();
}

var x = 10;

a();
```

Here JavaScript searches:

```
c() -> a() -> Global
```

Eventually it finds `x` in the global scope.

---

# Key Takeaways

- **Scope** determines where variables can be accessed
- **Lexical Environment = Local Memory + Parent Environment Reference**
- JavaScript uses **lexical (static) scope**
- Inner functions can access variables from outer functions
- Outer scopes cannot access variables inside inner functions
- JavaScript searches variables through the **Scope Chain**
- If the variable is not found, a **ReferenceError** occurs