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



