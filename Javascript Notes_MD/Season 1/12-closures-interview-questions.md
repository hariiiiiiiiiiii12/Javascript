# Episode 12: Famous Interview Questions on Closures

Closures are one of the most frequently asked topics in JavaScript interviews.  
Below are some common questions along with explanations.

---

# Q1: What is a Closure in JavaScript?

**Answer**

A **closure** is created when a function is bundled together with its **lexical environment**.

In other words:

> A closure is a combination of a function and the variables from its outer scope that the function remembers.

Example:

```javascript
function outer() {
    var a = 10;

    function inner() {
        console.log(a);
    }

    return inner;
}

outer()(); // 10
```

Explanation:

- `inner()` forms a closure with its outer function `outer()`
- The inner function remembers the variable `a`
- Even after `outer()` finishes execution, `inner()` still has access to `a`

---

# Q2: Will the following code still form a closure?

```javascript
function outer() {

    function inner() {
        console.log(a);
    }

    var a = 10;

    return inner;
}

outer()(); // 10
```

**Answer**

Yes.

Closures depend on **lexical scope**, not the order of statements.

Even though `a` is declared after `inner`, the inner function still forms a closure with the outer environment.

---

# Q3: If `var` is replaced with `let`, will the behavior change?

```javascript
function outer() {
    let a = 10;

    function inner() {
        console.log(a);
    }

    return inner;
}

outer()(); // 10
```

**Answer**

No.

Closures work the same way with `let` or `var`.

The inner function still remembers the variable `a`.

---

# Q4: Can the inner function access arguments of the outer function?

```javascript
function outer(str) {

    let a = 10;

    function inner() {
        console.log(a, str);
    }

    return inner;
}

outer("Hello There")(); // 10 "Hello There"
```

**Answer**

Yes.

The inner function forms a closure with:

- variable `a`
- argument `str`

Both belong to the outer scope.

---

# Q5: Will the inner function access variables from even higher scopes?

```javascript
function outest() {

    var c = 20;

    function outer(str) {

        let a = 10;

        function inner() {
            console.log(a, c, str);
        }

        return inner;
    }

    return outer;
}

outest()("Hello There")(); // 10 20 "Hello There"
```

**Answer**

Yes.

Closures allow functions to access **all outer scopes**.

Scope chain here:

```
inner → outer → outest → global
```

---

# Q6: What happens if a variable with the same name exists in global scope?

```javascript
function outest() {

    var c = 20;

    function outer(str) {

        let a = 10;

        function inner() {
            console.log(a, c, str);
        }

        return inner;
    }

    return outer;
}

let a = 100;

outest()("Hello There")(); // 10 20 "Hello There"
```

**Answer**

The output remains the same.

Why?

Because JavaScript resolves variables using the **scope chain**.

Search order:

```
inner → outer → outest → global
```

The nearest variable `a` is found inside `outer`, so the global `a = 100` is ignored.

---

# Q7: Advantages of Closures

Closures are useful in many patterns:

- Module Design Pattern
- Currying
- Memoization
- Data Hiding
- Encapsulation
- Asynchronous programming (setTimeout, callbacks)

---

# Q8: Data Hiding and Encapsulation using Closures

### Without closures

```javascript
var count = 0;

function increment() {
    count++;
}
```

Problem:

Any code can modify `count`.

---

### With closures

```javascript
function counter() {

    var count = 0;

    return function increment() {
        count++;
        console.log(count);
    };

}

var counter1 = counter();

counter1(); // 1
counter1(); // 2
```

Here:

- `count` becomes **private**
- Only the returned function can modify it

---

### Multiple independent counters

```javascript
var counter1 = counter();
counter1();

var counter2 = counter();
counter2();
```

Each call creates a **new closure with its own memory**.

---

# Using Constructor Pattern

For scalable implementations, constructors are often used.

```javascript
function Counter() {

    var count = 0;

    this.incrementCounter = function () {
        count++;
        console.log(count);
    };

    this.decrementCounter = function () {
        count--;
        console.log(count);
    };

}

var counter1 = new Counter();

counter1.incrementCounter();
counter1.incrementCounter();
counter1.decrementCounter();
```

Output:

```
1
2
1
```

Here `count` is private but accessible through methods.

---

# Q9: Disadvantages of Closures

Closures can also have drawbacks.

### Memory usage

Variables captured by closures **cannot be garbage collected** until the closure itself is removed.

Example:

```javascript
function a() {

    var x = 0;

    return function b() {
        console.log(x);
    };

}

var y = a();

y();
```

Here:

- `x` remains in memory
- Because function `b` still references it

If many closures are created unnecessarily, it may lead to **higher memory consumption**.

---

# Garbage Collector

A **garbage collector** is responsible for freeing unused memory.

In JavaScript:

- The engine automatically performs garbage collection
- Variables that are no longer referenced are removed from memory

Modern engines like **V8** optimize closure memory usage efficiently.

---

# Key Takeaways

- Closures allow functions to remember variables from outer scopes
- They rely on **lexical scoping**
- Closures are heavily used in modern JavaScript patterns
- They enable **data privacy and encapsulation**
- Improper use of closures may lead to **memory overhead**