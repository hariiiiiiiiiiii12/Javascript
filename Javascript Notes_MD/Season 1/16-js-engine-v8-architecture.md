# Episode 16: JS Engine Exposed – Google's V8 Architecture

JavaScript can run almost everywhere today:

- Browsers
- Servers (Node.js)
- Mobile devices
- Smart watches
- Robots
- Embedded systems

This is possible because of the **JavaScript Runtime Environment (JRE)**.

---

# JavaScript Runtime Environment (JRE)

The **JavaScript Runtime Environment** is the environment required to run JavaScript code.

Think of it as a container that contains everything required to execute JavaScript.

A typical JRE consists of:

- JavaScript Engine
- Web APIs
- Event Loop
- Callback Queue
- Microtask Queue
- Other runtime components

The **JavaScript Engine** is the most important part of the runtime environment.

---

# Why Browsers Can Run JavaScript

Browsers contain a **JavaScript Runtime Environment**.

This allows them to:

- Parse JavaScript code
- Compile it
- Execute it
- Interact with browser APIs

Without the runtime environment, JavaScript code would not be able to run.

---

# ECMAScript

**ECMAScript** is the official specification for JavaScript.

It defines the rules and standards that all JavaScript engines must follow.

Different browsers implement these rules using their own engines.

Examples of JavaScript engines:

| Browser | Engine |
|-------|-------|
| Chrome | V8 |
| Firefox | SpiderMonkey |
| Edge (old) | Chakra |
| Safari | JavaScriptCore |

SpiderMonkey was the **first JavaScript engine**, created by Brendan Eich (the creator of JavaScript).

---

# What is a JavaScript Engine?

A **JavaScript Engine** is software written in low-level languages such as **C++**.

Its job is to:

1. Take high-level JavaScript code
2. Convert it into machine code
3. Execute it efficiently on the system

---

# Stages of JavaScript Code Execution

JavaScript code goes through three major stages:

1. Parsing  
2. Compilation  
3. Execution

---

# 1. Parsing

During parsing, the JavaScript code is broken down into **tokens**.

Example:

```javascript
let a = 7;
```

Tokens would be:

```
let
a
=
7
```

After tokenization, a **Syntax Parser** converts the code into an **Abstract Syntax Tree (AST)**.

An AST is a tree-like structure that represents the syntactic structure of the program.

Developers can explore ASTs using tools like:

```
https://astexplorer.net
```

The AST becomes the internal representation used by the JavaScript engine.

---

# 2. Compilation

Modern JavaScript engines use **Just-In-Time (JIT) Compilation**.

JIT combines the advantages of:

- Interpreters
- Compilers

Process:

1. The AST is passed to an **Interpreter**
2. The interpreter converts the code into **bytecode**
3. While the program runs, a **compiler optimizes frequently executed code**

This allows JavaScript engines to improve performance dynamically during execution.

Earlier JavaScript engines were purely interpreted.  
Modern engines combine **interpretation and compilation**.

---

# 3. Execution

Execution requires two main components:

- **Memory Heap**
- **Call Stack**

### Memory Heap

The memory heap is where all variables, objects, and functions are stored.

### Call Stack

The call stack keeps track of function execution.

It manages:

- Global execution context
- Function execution contexts

---

# Garbage Collection

JavaScript engines automatically manage memory using a **Garbage Collector**.

Garbage collection removes memory that is no longer being used.

One commonly used algorithm is:

```
Mark and Sweep
```

Process:

1. Identify objects that are still reachable
2. Mark them
3. Remove all other unused objects from memory

---

# Google's V8 Engine

The **V8 engine** powers:

- Google Chrome
- Node.js
- Many other platforms

V8 contains several internal components.

Important ones include:

| Component | Purpose |
|---------|---------|
| Ignition | Interpreter |
| TurboFan | Optimizing Compiler |
| Orinoco | Garbage Collector |

---

# V8 Execution Flow

The simplified workflow of the V8 engine:

```
JavaScript Code
      ↓
Parsing
      ↓
Abstract Syntax Tree (AST)
      ↓
Ignition Interpreter
      ↓
Bytecode Execution
      ↓
TurboFan Compiler (Optimization)
      ↓
Machine Code Execution
```

The engine continuously optimizes frequently used code during runtime.

---

# Key Takeaways

- JavaScript runs everywhere because of the **JavaScript Runtime Environment**
- The **JavaScript Engine** is the core component responsible for executing code
- ECMAScript defines the standards that all JavaScript engines follow
- Code execution goes through **Parsing → Compilation → Execution**
- Modern JavaScript engines use **JIT compilation**
- Execution relies on **Memory Heap and Call Stack**
- Garbage collection automatically frees unused memory
- Google's **V8 engine** uses Ignition (interpreter) and TurboFan (compiler)