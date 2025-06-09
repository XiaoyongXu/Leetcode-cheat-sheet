# Leetcode-cheat-sheet
---

## Breadth-First Search (BFS)
**Concept:** Explores all the **neighbor nodes at the present depth** prior to moving on to the nodes at the next depth level. It uses a **queue** data structure to keep track of the nodes to visit.
**Implementation:**
```javascript
function bfs(graph, startNode) {
  const visited = new Set();
  const queue = [startNode];
  visited.add(startNode);

  while (queue.length > 0) {
    const node = queue.shift();
    console.log(node); // Process the node

    for (const neighbor of graph[node] || []) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push(neighbor);
      }
    }
  }
}
```
---
## Depth-First Search (DFS)
**Concept:** Explores as far as possible along each branch before backtracking. It can be implemented using **recursion** or a **stack**.
### Recursive Implementation
```javascript
function dfsRecursive(graph, node, visited = new Set()) {
  if (visited.has(node)) {
    return;
  }

  visited.add(node);
  console.log(node); // Process the node

  for (const neighbor of graph[node] || []) {
    dfsRecursive(graph, neighbor, visited);
  }
}
```
### Iterative Implementation
```javascript
function dfsIterative(graph, startNode) {
  const visited = new Set();
  const stack = [startNode];

  while (stack.length > 0) {
    const node = stack.pop();
    if (visited.has(node)) {
      continue;
    }

    visited.add(node);
    console.log(node); // Process the node

    for (const neighbor of graph[node] || []) {
      stack.push(neighbor);
    }
  }
}
```
---
## Example Graph Representation
```javascript
const graph = {
  'A': ['B', 'C'],
  'B': ['A', 'D', 'E'],
  'C': ['A', 'F'],
  'D': ['B'],
  'E': ['B', 'F'],
  'F': ['C', 'E']
};
```
---
## Dynamic Programming (DP) Cheat Sheet
**Concept:** Dynamic Programming is an algorithmic technique for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems just once, and storing their solutions. The next time the same subproblem occurs, you look up the previously computed solution instead of recomputing it.

---

### Memoization (Top-Down) Template

```javascript
const memo = {}; // Or new Map() for more complex keys

function solve(param1, param2, ...) {
    // 1. Check if already computed (Base Case / Memoization Hit)
    const key = `${param1}-${param2}`; // Create a unique key for the state
    if (memo[key] !== undefined) {
        return memo[key];
    }

    // 2. Define Base Cases for the recursion
    // if (param1 === 0 && param2 === 0) {
    //     return 0;
    // }
    // if (param1 < 0 || param2 < 0) {
    //     return Infinity; // Or appropriate value for invalid state
    // }

    // 3. Recurrence Relation (Recursive calls to smaller subproblems)
    // const result = ... solve(smaller_params1, smaller_params2) ...
    // Example: Fibonacci
    if (param1 <= 1) { // if solving fib(n), then n is param1
        return memo[key] = param1;
    }
    const result = solve(param1 - 1) + solve(param1 - 2);


    // 4. Store and return the result
    return memo[key] = result;
}

// Initial call
// console.log(solve(N, ...));
```
---
### Tabulation (Bottom-Up) Template
```javascript
function solveTabulation(size1, size2, ...) {
    // 1. Initialize DP table
    // For 1D: const dp = new Array(size + 1).fill(initialValue);
    // For 2D: const dp = Array.from({ length: size1 + 1 }, () => new Array(size2 + 1).fill(initialValue));
    const dp = new Array(size1 + 1);
    for (let i = 0; i <= size1; i++) {
        dp[i] = new Array(size2 + 1).fill(0); // Example initial value
    }

    // 2. Fill Base Cases
    // dp[0][0] = 0;
    // dp[1][0] = ...;

    // 3. Iterate through the DP table
    // Ensure correct order of iteration so dependencies are met
    for (let i = 0; i <= size1; i++) {
        for (let j = 0; j <= size2; j++) {
            // Apply recurrence relation
            // Example: Fibonacci (dp[i] = dp[i-1] + dp[i-2])
            if (i <= 1) { // for fib(i)
                dp[i][j] = i; // or dp[i] = i
                continue; // Move to next iteration or ensure base case doesn't interfere with recurrence
            }
            if (i > 1) {
                dp[i][j] = dp[i-1][j] + dp[i-2][j]; // Example using 2D for illustration, apply to correct dimension
            }
        }
    }

    // 4. Return the final result
    return dp[size1][size2]; // Or dp[size1] for 1D
}

// Initial call
// console.log(solveTabulation(N, M, ...));
```
---

# 🧵 Singly Linked List in JavaScript
## 📌 1. Define a Node

```js
class ListNode {
  constructor(value) {
    this.value = value;
    this.next = null;
  }
}
```
---

## 📌 2. Define the LinkedList Class

```js
class LinkedList {
  constructor() {
    this.head = null;
  }

  // Add node at the end
  append(value) {
    const newNode = new ListNode(value);
    if (!this.head) {
      this.head = newNode;
      return;
    }

    let current = this.head;
    while (current.next) {
      current = current.next;
    }
    current.next = newNode;
  }

  // Add node at the beginning
  prepend(value) {
    const newNode = new ListNode(value);
    newNode.next = this.head;
    this.head = newNode;
  }

  // Print the list
  print() {
    let current = this.head;
    const values = [];
    while (current) {
      values.push(current.value);
      current = current.next;
    }
    console.log(values.join(" -> "));
  }

  // Delete a node by value
  delete(value) {
    if (!this.head) return;

    if (this.head.value === value) {
      this.head = this.head.next;
      return;
    }

    let current = this.head;
    while (current.next && current.next.value !== value) {
      current = current.next;
    }

    if (current.next) {
      current.next = current.next.next;
    }
  }
}
```
---
## ✅ 3. Example Usage

```js
const list = new LinkedList();

list.append(10);
list.append(20);
list.append(30);
list.prepend(5);

list.print(); // Output: 5 -> 10 -> 20 -> 30

list.delete(20);

list.print(); // Output: 5 -> 10 -> 30
```
---
Here’s a clean Markdown reference for implementing a **Stack** and **Queue** in JavaScript:

---

# 📚 Stack and Queue in JavaScript

---

## 🥞 Stack (LIFO - Last In, First Out)

### ✅ Using Array (built-in)

```js
const stack = [];

// Push
stack.push(10);
stack.push(20);

// Pop
const top = stack.pop(); // 20

// Peek
const peek = stack[stack.length - 1]; // 10

// Check empty
const isEmpty = stack.length === 0;
```

---

## 🧃 Queue (FIFO - First In, First Out)

### ✅ Using Array (inefficient `shift()`)

```js
const queue = [];

// Enqueue
queue.push(10);
queue.push(20);

// Dequeue
const first = queue.shift(); // 10

// Peek
const peek = queue[0];

// Check empty
const isEmpty = queue.length === 0;
```



