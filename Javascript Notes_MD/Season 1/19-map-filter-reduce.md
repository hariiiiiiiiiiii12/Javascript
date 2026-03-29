# Episode 19: map, filter & reduce

`map`, `filter`, and `reduce` are **Higher-Order Functions** commonly used in functional programming with arrays.

They help process arrays in a **declarative and reusable way**.

---

# map()

The `map()` function is used to **transform each element of an array**.

It creates a **new array** by applying a transformation function to every element.

Syntax:

```javascript
const output = arr.map(function);
```

The function passed to `map()` defines **how each element should be transformed**.

---

## Example 1: Double each element

```javascript
const arr = [5, 1, 3, 2, 6];

function double(x) {
  return x * 2;
}

const doubleArr = arr.map(double);

console.log(doubleArr); 
// [10, 2, 6, 4, 12]
```

---

## Example 2: Triple each element

```javascript
const arr = [5, 1, 3, 2, 6];

function triple(x) {
  return x * 3;
}

const tripleArr = arr.map(triple);

console.log(tripleArr);
// [15, 3, 9, 6, 18]
```

---

## Example 3: Convert numbers to binary

```javascript
const arr = [5, 1, 3, 2, 6];

function binary(x) {
  return x.toString(2);
}

const binaryArr = arr.map(binary);

console.log(binaryArr);
```

Alternative implementations:

```javascript
const binaryArr = arr.map(function(x) {
  return x.toString(2);
});
```

Using arrow functions:

```javascript
const binaryArr = arr.map((x) => x.toString(2));
```

---

# filter()

The `filter()` function is used to **select elements that satisfy a condition**.

It returns a **new array containing only the elements that pass the condition**.

Syntax:

```javascript
arr.filter(function);
```

---

## Example: Filter odd numbers

```javascript
const arr = [5, 1, 3, 2, 6];

function isOdd(x) {
  return x % 2;
}

const oddArr = arr.filter(isOdd);

console.log(oddArr);
// [5, 1, 3]
```

Arrow function version:

```javascript
const oddArr = arr.filter((x) => x % 2);
```

---

# reduce()

The `reduce()` function processes all elements of an array and **reduces them to a single value**.

Syntax:

```javascript
arr.reduce(function(accumulator, currentValue), initialValue);
```

- **Accumulator** stores the accumulated result.
- **Current value** represents the current array element.

---

## Example: Sum of array elements

Non-functional approach:

```javascript
const arr = [5, 1, 3, 2, 6];

function findSum(arr) {
  let sum = 0;

  for (let i = 0; i < arr.length; i++) {
    sum += arr[i];
  }

  return sum;
}

console.log(findSum(arr));
// 17
```

Using `reduce()`:

```javascript
const arr = [5, 1, 3, 2, 6];

const sumOfElem = arr.reduce(function(acc, curr) {
  acc = acc + curr;
  return acc;
}, 0);

console.log(sumOfElem);
// 17
```

---

## Example: Find maximum element

Non-functional approach:

```javascript
const arr = [5, 1, 3, 2, 6];

function findMax(arr) {
  let max = 0;

  for (let i = 0; i < arr.length; i++) {
    if (arr[i] > max) {
      max = arr[i];
    }
  }

  return max;
}

console.log(findMax(arr));
// 6
```

Using `reduce()`:

```javascript
const arr = [5, 1, 3, 2, 6];

const output = arr.reduce((max, curr) => {
  if (curr > max) {
    max = curr;
  }
  return max;
}, 0);

console.log(output);
// 6
```

---

# Tricky map() Example

```javascript
const users = [
  { firstName: "Alok", lastName: "Raj", age: 23 },
  { firstName: "Ashish", lastName: "Kumar", age: 29 },
  { firstName: "Ankit", lastName: "Roy", age: 29 },
  { firstName: "Pranav", lastName: "Mukherjee", age: 50 }
];

const fullNameArr = users.map(
  (user) => user.firstName + " " + user.lastName
);

console.log(fullNameArr);
```

Output:

```
["Alok Raj", "Ashish Kumar", "Ankit Roy", "Pranav Mukherjee"]
```

---

# Using reduce() to Generate Reports

Example: Count users by age.

```javascript
const users = [
  { firstName: "Alok", lastName: "Raj", age: 23 },
  { firstName: "Ashish", lastName: "Kumar", age: 29 },
  { firstName: "Ankit", lastName: "Roy", age: 29 },
  { firstName: "Pranav", lastName: "Mukherjee", age: 50 }
];

const report = users.reduce((acc, curr) => {

  if (acc[curr.age]) {
    acc[curr.age] = acc[curr.age] + 1;
  } else {
    acc[curr.age] = 1;
  }

  return acc;

}, {});

console.log(report);
```

Output example:

```
{ 23: 1, 29: 2, 50: 1 }
```

---

# Function Chaining

Multiple higher-order functions can be chained together.

Example: Get first names of users whose age is less than 30.

```javascript
const users = [
  { firstName: "Alok", lastName: "Raj", age: 23 },
  { firstName: "Ashish", lastName: "Kumar", age: 29 },
  { firstName: "Ankit", lastName: "Roy", age: 29 },
  { firstName: "Pranav", lastName: "Mukherjee", age: 50 }
];

const output = users
  .filter((user) => user.age < 30)
  .map((user) => user.firstName);

console.log(output);
// ["Alok", "Ashish", "Ankit"]
```

---

# Same Logic Using reduce()

```javascript
const output = users.reduce((acc, curr) => {

  if (curr.age < 30) {
    acc.push(curr.firstName);
  }

  return acc;

}, []);

console.log(output);
// ["Alok", "Ashish", "Ankit"]
```

---

# Key Takeaways

- `map()` transforms each element of an array.
- `filter()` selects elements based on a condition.
- `reduce()` processes all elements to produce a single value.
- These functions support **functional programming patterns**.
- They improve **readability, reusability, and maintainability** of code.