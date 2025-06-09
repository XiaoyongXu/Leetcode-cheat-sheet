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

**Use Cases:**

* Finding the **shortest path in an unweighted graph**.
* Exploring nodes level by level.
* Finding all connected components in a graph.

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

**Use Cases:**

* Checking for **cycles** in a graph.
* Topological sorting.
* Solving puzzles with backtracking.

---

## Key Differences

* **Data Structure:** BFS uses a **queue**, while DFS uses a **stack** (implicitly with recursion or explicitly).
* **Traversal Order:** BFS explores level by level, while DFS explores depth first.
* **Shortest Path:** BFS guarantees the shortest path in unweighted graphs, while DFS does not.
* **Memory Usage:** BFS can use more memory for wide graphs, while DFS uses less memory for deep graphs.

---

## When to Use

* **BFS:** When you need to find the **shortest path in unweighted graphs** or explore nodes level by level.
* **DFS:** When you need to **explore deep into the graph** or check for cycles.

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

## Additional Notes

* Both BFS and DFS can be used to traverse trees as well as graphs.
* The choice between BFS and DFS depends on the specific problem you are trying to solve.
* These implementations use a `Set` to keep track of visited nodes, which ensures that each node is visited only once.


---
Yes, I can definitely create a similar cheat sheet for Dynamic Programming (DP)!

Dynamic Programming is a powerful technique often used to solve complex problems by breaking them down into simpler subproblems. Here's a cheat sheet covering its core concepts, common patterns, and implementation considerations.

---

## Dynamic Programming (DP) Cheat Sheet

**Concept:** Dynamic Programming is an algorithmic technique for solving a complex problem by breaking it down into a collection of simpler subproblems, solving each of those subproblems just once, and storing their solutions. The next time the same subproblem occurs, you look up the previously computed solution instead of recomputing it.

**Two Main Approaches:**

1.  **Memoization (Top-Down DP):**
    * Starts from the original problem and recursively breaks it down.
    * Stores the results of expensive function calls in a cache (e.g., a hash map or array) and returns the cached result when the same inputs occur again.
    * Often more intuitive to write, as it mirrors the recursive definition of the problem.
    * Might visit states that are not strictly necessary if some subproblems are never called.

2.  **Tabulation (Bottom-Up DP):**
    * Starts by solving the smallest subproblems first and iteratively builds up solutions to larger subproblems.
    * Typically uses an array (DP table) to store solutions.
    * Requires careful ordering of subproblems to ensure dependencies are met.
    * Guaranteed to compute all necessary subproblems and often has better constant factors for space and time.

---

### Key Components of a DP Solution

1.  **Optimal Substructure:**
    * A problem has optimal substructure if an optimal solution to the problem can be constructed from optimal solutions to its subproblems.
    * Example: The shortest path between two nodes includes the shortest path to an intermediate node.

2.  **Overlapping Subproblems:**
    * The problem can be broken down into subproblems that are reused multiple times.
    * Without overlapping subproblems, a simple recursion (without memoization) would be sufficient.

3.  **State Definition:**
    * Crucial step: How do you define `DP[i]`, `DP[i][j]`, etc.? What does each entry in your DP table represent?
    * This definition should capture all necessary information to solve future subproblems.

4.  **Base Cases:**
    * The simplest subproblems whose solutions are known without further recursion or calculation. These are the starting points for your DP table.

5.  **Recurrence Relation (Transition):**
    * How do you compute the solution for a larger subproblem from the solutions of smaller subproblems?
    * This is the core formula that defines how `DP[current_state]` is derived from `DP[previous_states]`.

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

### Common DP Patterns / Problem Types

1.  **Fibonacci Sequence:**
    * `dp[i] = dp[i-1] + dp[i-2]`
    * Classic example of overlapping subproblems.

2.  **Climbing Stairs (or LeetCode 70: Climbing Stairs):**
    * Similar to Fibonacci. `dp[i]` is number of ways to reach `i`.
    * `dp[i] = dp[i-1] + dp[i-2]` (if you can take 1 or 2 steps).

3.  **Coin Change (LeetCode 322: Coin Change):**
    * **Minimum Coins:** `dp[amount] = min(dp[amount - coin] + 1)` for all `coin` in `coins`.
    * **Number of Ways:** `dp[amount] += dp[amount - coin]` (careful with order for combinations vs. permutations).

4.  **Knapsack Problem:**
    * **0/1 Knapsack (LeetCode 416: Partition Equal Subset Sum, LeetCode 494: Target Sum):** Each item can be taken or not taken.
        * `dp[i][w]` represents max value using first `i` items with max weight `w`.
        * `dp[i][w] = max(dp[i-1][w], dp[i-1][w - weight[i]] + value[i])`
    * **Unbounded Knapsack:** Items can be taken multiple times.

5.  **Longest Common Subsequence (LCS) / Edit Distance:**
    * `dp[i][j]` typically represents something about the prefix of string1 of length `i` and prefix of string2 of length `j`.
    * `if (str1[i-1] === str2[j-1]): dp[i][j] = dp[i-1][j-1] + 1`
    * `else: dp[i][j] = max(dp[i-1][j], dp[i][j-1])` (for LCS)
    * **Edit Distance (LeetCode 72: Edit Distance):** Involves insert, delete, replace operations.

6.  **Matrix Chain Multiplication:**
    * Optimization problem for parentheses placement.

7.  **Subarray/Subsequence Problems:**
    * **Maximum Subarray Sum (Kadane's Algorithm - a form of DP):** `dp[i] = max(nums[i], nums[i] + dp[i-1])`
    * **Longest Increasing Subsequence (LIS):** `dp[i]` is the length of the LIS ending at index `i`.
        * `dp[i] = 1 + max(dp[j])` for all `j < i` where `nums[j] < nums[i]`.

8.  **Grid Problems (e.g., Unique Paths, Minimum Path Sum):**
    * `dp[i][j]` is the solution at cell `(i, j)`.
    * `dp[i][j] = dp[i-1][j] + dp[i][j-1]` (for unique paths)
    * `dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1])` (for minimum path sum)

---

### Optimization Techniques

* **Space Optimization:**
    * Sometimes `dp[i]` only depends on `dp[i-1]` and `dp[i-2]`. You can reduce space from `O(N)` to `O(1)` by only storing the last few values.
    * For 2D DP, if `dp[i][j]` only depends on `dp[i-1]`, you might reduce space from `O(N*M)` to `O(M)` by only keeping two rows/columns.

* **State Compression:**
    * Using bitmasks to represent subsets or visited states, especially for problems with small `N` (e.g., Traveling Salesperson Problem on small graphs).

---

### General Tips for DP Problems

1.  **Identify if it's a DP problem:** Look for optimal substructure and overlapping subproblems.
2.  **Define the State:** This is the most crucial step. What information do you need to carry forward?
3.  **Write down the Recurrence Relation:** How does the current state depend on previous states?
4.  **Identify Base Cases:** What are the smallest, known solutions?
5.  **Choose Memoization or Tabulation:**
    * **Memoization:** Often easier to implement initially for complex recursive definitions. Good for when you don't need to compute all subproblems (e.g., only a specific path).
    * **Tabulation:** Usually more efficient (no recursion overhead), easier to space optimize, and good for problems where you need to compute all subproblems.
6.  **Consider the Order of Iteration (for Tabulation):** Ensure that when you compute `dp[i]`, all its dependencies (`dp[j]` where `j < i`) are already computed.
7.  **Draw a DP Table (for Tabulation):** Sometimes visualizing the table helps in understanding the transitions and base cases.
8.  **Trace an Example:** Walk through a small example to verify your recurrence relation and base cases.

---

This cheat sheet should give you a strong foundation for approaching and solving Dynamic Programming problems!


