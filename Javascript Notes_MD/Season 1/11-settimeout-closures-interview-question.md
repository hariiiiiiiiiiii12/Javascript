# Episode 11: setTimeout + Closures Interview Question

> Time, tide and JavaScript wait for none.

This topic is a very common **JavaScript interview question** that tests understanding of:

- Closures
- setTimeout
- Event Loop
- Scope

---

# Basic Example

```javascript
function x() {
    var i = 1;

    setTimeout(function () {
        console.log(i);
    }, 3000);

    console.log("Namaste Javascript");
}

x();
```

Output:

```
Namaste Javascript
1   // printed after 3 seconds
```

---

# Why Does This Happen?

At first glance, many people expect JavaScript to:

1. Wait 3 seconds
2. Print `1`
3. Then print `"Namaste Javascript"`

But JavaScript actually does the opposite.

### What really happens

1. Function `x()` starts executing
2. `setTimeout` receives a **callback function**
3. JavaScript registers this callback with a **timer of 3000ms**
4. JavaScript **does not wait** for the timer
5. Execution continues immediately
6. `"Namaste Javascript"` is printed
7. After 3 seconds, the callback function is pushed into the **Call Stack**
8. The callback executes and prints `1`

---

# Important Concept

The function inside `setTimeout` forms a **closure**.

It remembers the **reference to variable `i`** from its lexical scope.

---

# Classic Interview Question

Print numbers like this:

```
1 after 1 second
2 after 2 seconds
3 after 3 seconds
4 after 4 seconds
5 after 5 seconds
```

A beginner might write this:

```javascript
function x() {

    for (var i = 1; i <= 5; i++) {

        setTimeout(function () {
            console.log(i);
        }, i * 1000);

    }

    console.log("Namaste Javascript");
}

x();
```

Output:

```
Namaste Javascript
6
6
6
6
6
```

---

# Why Does It Print 6?

This happens because of **closures**.

When `setTimeout` stores the callback function:

- It remembers the **reference to variable `i`**
- Not the value of `i`

During the loop:

```
i → 1
i → 2
i → 3
i → 4
i → 5
```

But the loop completes **very fast**.

After the loop ends:

```
i = 6
```

When the callbacks finally execute, they all read the **same variable `i`**, whose value is now `6`.

That is why `6` gets printed five times.

---

# Solution 1: Using let

```javascript
function x() {

    for (let i = 1; i <= 5; i++) {

        setTimeout(function () {
            console.log(i);
        }, i * 1000);

    }

    console.log("Namaste Javascript");
}

x();
```

Output:

```
Namaste Javascript
1
2
3
4
5
```

### Why does this work?

Because `let` is **block scoped**.

Each loop iteration creates a **new copy of `i`**.

So each callback function closes over a **different variable**.

---

# Solution 2: Using var with Closures

Sometimes interviewers ask you to solve this **without using let**.

You can create a new function scope.

```javascript
function x() {

    for (var i = 1; i <= 5; i++) {

        function close(i) {

            setTimeout(function () {
                console.log(i);
            }, i * 1000);

        }

        close(i);

    }

    console.log("Namaste Javascript");
}

x();
```

### Why does this work?

Each call to `close(i)` creates:

- a **new execution context**
- a **new variable `i`**

Now each callback function forms a **closure with its own copy of `i`**.

So the numbers print correctly.

---

# Key Concepts Tested in Interviews

This question checks your understanding of:

- Closures
- setTimeout behavior
- Event loop
- Callback queue
- Block scope (`let`)
- Function scope (`var`)

If you understand this problem deeply, you understand **how asynchronous JavaScript works internally**.