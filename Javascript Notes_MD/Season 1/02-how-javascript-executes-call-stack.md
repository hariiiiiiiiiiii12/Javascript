# Episode 2: How JavaScript Is Executed & The Call Stack

Whenever a JavaScript program starts running, the JS engine first creates something called the **Global Execution Context (GEC)**.

An **Execution Context** is simply the environment in which JavaScript code is evaluated and executed.

Think of it like a container where JavaScript stores variables, functions and executes code.

---

# Phases of Execution Context

Every execution context is created in **two phases**:

1. Memory Creation Phase
2. Code Execution Phase

---

# 1. Memory Creation Phase

Before executing the code, JavaScript scans the entire program and allocates memory for variables and functions.

During this phase:

- Variables are allocated memory and initialized with the value **undefined**
- Functions are allocated memory and **their entire function definition is stored**

No code actually runs in this phase. JavaScript is just preparing memory.

---

# Example Code

```javascript
var n = 2;

function square(num) {
  var ans = num * num;
  return ans;
}

var square2 = square(n);
var square4 = square(4);
```

---

# What Happens During Memory Creation

JavaScript scans the entire code first and prepares memory like this:

```
n: undefined

square: function square(num) {
  var ans = num * num;
  return ans;
}

square2: undefined
square4: undefined
```

At this stage:

- `n` exists but its value is **undefined**
- `square` stores the **entire function definition**
- `square2` and `square4` are **undefined**

No code has executed yet.

---

# 2. Code Execution Phase

After memory allocation is complete, JavaScript starts executing the code **line by line**.

### Step 1

```javascript
var n = 2;
```

Now the value `2` is assigned to `n`.

```
n = 2
```

---

### Step 2

```javascript
function square(num) { ... }
```

Functions were already stored during the memory phase, so **nothing happens here during execution**.

---

### Step 3

```javascript
var square2 = square(n);
```

Here JavaScript encounters a **function call**.

Whenever a function is invoked, JavaScript creates a **new Execution Context** for that function.

So a **Function Execution Context** is created for `square()`.

---

# Inside the square() Execution Context

Again the same **two phases occur**.

---

## Memory Creation Phase

Memory is allocated for the function variables.

```
num: undefined
ans: undefined
```

---

## Code Execution Phase

Now the function code starts executing.

### Step 1

```
num = 2
```

Because `square(n)` was called and `n` was `2`.

---

### Step 2

```javascript
var ans = num * num;
```

```
ans = 4
```

---

### Step 3

```javascript
return ans;
```

The function returns `4`.

Control goes back to the line where the function was called.

```
var square2 = square(n);
```

So now:

```
square2 = 4
```

After returning, the **function execution context is deleted**.

---

# Next Function Call

```javascript
var square4 = square(4);
```

The same process repeats.

Inside this execution:

```
num = 4
ans = 16
```

Return value:

```
square4 = 16
```

Once the function finishes, that execution context is also **removed**.

---

# Final Global Memory State

After all code execution completes:

```
n = 2
square = function definition
square2 = 4
square4 = 16
```

Once the program finishes running, the **Global Execution Context is also destroyed**.

---

# The Call Stack

JavaScript manages execution contexts using something called the **Call Stack**.

The Call Stack keeps track of **which execution context is currently running**.

It follows the **Last In First Out (LIFO)** principle.

---

# How the Call Stack Works

### When the program starts

The **Global Execution Context** is pushed onto the stack.

```
Call Stack
-----------
Global()
```

---

### When square(n) is called

A new execution context is pushed.

```
Call Stack
-----------
square()
Global()
```

---

### After square(n) finishes

It is popped from the stack.

```
Call Stack
-----------
Global()
```

---

### When square(4) is called

```
Call Stack
-----------
square()
Global()
```

---

### After execution

```
Call Stack
-----------
Global()
```

---

### When the program finishes

```
Call Stack
-----------
(empty)
```

---

# Other Names for Call Stack

The Call Stack is also known as:

- Program Stack
- Control Stack
- Runtime Stack
- Machine Stack
- Execution Context Stack

All of them refer to the same concept.

---

# Key Takeaways

- JavaScript runs inside **Execution Contexts**
- The first execution context created is the **Global Execution Context**
- Every execution context has two phases:
  - Memory Creation Phase
  - Code Execution Phase
- Variables are initialized with **undefined** during the memory phase
- Functions store their **entire definition in memory**
- Every function call creates a **new execution context**
- Execution contexts are managed using the **Call Stack**