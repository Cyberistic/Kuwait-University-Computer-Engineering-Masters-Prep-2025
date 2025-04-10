Trees and graphs: Binary trees; binary tree representation; binary tree traversal - inorder, preorder, and postorder, binary tree representation of trees, heaps, graph representation, graph traversals, shortest path, complexities of operations in trees and graphs.

Before you get started, make sure you remember the following:

1. [[Calculating Height & Width of a Tree]]
2. The meaning of `root`, `child`, `leaf`, `edge`, `siblings`
3. An **ordered tree** is a tree where the **children of each node have a specific order** — like first child, second child, third child, and so on.
   We usually show this by drawing the children **from left to right** in the order they belong.

## Trees and Graphs

### Binary Trees

```mermaid
graph TD
  A((1)) --> B((2))
  A --> C((3))
  B --> D((4))
  B --> E((5))
  E --> H((8))
  E --> I((9))
  C --> F((6))
  C --> G((7))
  G --> J((10))
```

A **binary tree** is an ordered tree with the following properties:

> 1. Every node has at most two children.
> 2. Each child node is labeled as being either a left child or a right child.
> 3. A left child precedes a right child in the order of children of a node

```ts
type TreeNode = {
  id: string;
  value: number;
  sibling: TreeNode | null; // optionally store your sibling
  left: TreeNode | null;
  right: TreeNode | null;
};
```

More boring facts:

- The **root** node is the entry point. Leaves are nodes with no children.
- A binary tree is _proper_ if each node has either zero or two children.
- _The recursive definition:_ a binary tree has a root, the root has 0 to 2 binary trees as children (left subtree, right subtree)

> [!tip] Binary Search Trees  
> A **Binary Search Tree (BST)** is a special kind of binary tree where:
>
> - Left child < parent
> - Right child > parent  
>    Useful for fast lookup, insertion, and deletion.
>    With a time complexity of **O(log n)** in the **average** and **best** cases.
  
#### Binary Tree Traversals

Tree traversals are ways to "walk through" the tree. There are three main types:

### 1. Inorder (Left → Root → Right)

```ts
function inorder(node: TreeNode | null) {
  if (!node) return;
  inorder(node.left);
  console.log(node.value);
  inorder(node.right);
}
```

> [!tip] In BSTs, inorder gives you values in **sorted order**.
> A way of sorting an array of numbers is to push its elements into a new BST and then traverse and pop them back using inorder traversal! :D

### 2. Preorder (Root → Left → Right)

```ts
function preorder(node: TreeNode | null) {
  if (!node) return;
  console.log(node.value);
  preorder(node.left);
  preorder(node.right);
}
```

### 3. Postorder (Left → Right → Root)

```ts
function postorder(node: TreeNode | null) {
  if (!node) return;
  postorder(node.left);
  postorder(node.right);
  console.log(node.value);
}
```

> [!warning] I left out adding and removing nodes as you're probably familiar with how its done by now (same concepts as lists)
---

#### Normal Tree → Binary Tree Representation

_Prepare for a headache, this is a bit confusing but bear with me_

Any general tree (where a node can have more than 2 children) can be represented as a **binary tree** using the **Left-Child Right-Sibling** technique:

- Left child → first child
- Right child → next sibling

In other non-confusing words:

start with root:

- The first child (let's call it `1st`) of a node stays in the same place on the left.
- The next child (`2nd`) becomes the right child of `1st`.
- The next child (`3rd`) becomes the right child of `2nd`.
  and so on.. once there are no more children, do the same for the other nodes, starting with `1st`.

Let's see an example:

Original tree:

```mermaid
graph TD
    A((5)) --> B((8))
    A --> C((2))
    A --> D((1))
    B --> E((3))
    B --> F((4))
```

Converted to binary tree using Left-Child Right-Sibling representation:

```mermaid
graph TD
    A((5)) --> B((8))
    B --> E((3))
    B --> C((2))
    C --> D((1))
    E --> F((4))
```

Notice how:

1. B (first child of A) stays as left child of A
2. C (second child of A) becomes right child of B
3. D (third child of A) becomes right child of C
4. E (first child of B) stays as left child of B
5. F (second child of B) becomes right child of E

Confusing right? ahahah will probably be in the exam.
I would personally just keep 


## Heaps

A **Heap** is a complete binary tree that satisfies the **heap property**.

- **Min-Heap**: Parent is less than both children

- **Max-Heap**: Parent is greater than both children

Heaps are often stored as arrays.

### Example: Max-Heap stored in array

```

Index: 0 1 2 3 4
Value: [50, 30, 40, 10, 20]

```

```ts
const leftChild = (i: number) => 2 * i + 1;
const rightChild = (i: number) => 2 * i + 2;
const parent = (i: number) => Math.floor((i - 1) / 2);
```

> [!tip] Use Heaps for Priority Queues  
> Insertion and deletion: `O(log n)`

---

## Graphs

A **graph** is a set of **nodes (vertices)** and **edges** connecting them.

```ts
type Graph = {
  [node: string]: string[]; // adjacency list representation
};
```

### Types

- **Directed** vs **Undirected**
- **Weighted** vs **Unweighted**
- **Cyclic** vs **Acyclic**
- **Connected** vs **Disconnected**

---

## Graph Representations

### 1. Adjacency Matrix

```ts
const matrix = [
  [0, 1, 0],
  [1, 0, 1],
  [0, 1, 0]
];
```

- Good for dense graphs
- Space: `O(n^2)`

### 2. Adjacency List

```ts
const adjList = {
  A: ["B"],
  B: ["A", "C"],
  C: ["B"]
};
```

- Good for sparse graphs
- Space: `O(V + E)`

---

## Graph Traversals

### 1. Depth-First Search (DFS)

```ts
function dfs(graph: Graph, start: string, visited = new Set()) {
  if (visited.has(start)) return;
  visited.add(start);
  console.log(start);
  for (const neighbor of graph[start]) {
    dfs(graph, neighbor, visited);
  }
}
```

### 2. Breadth-First Search (BFS)

```ts
function bfs(graph: Graph, start: string) {
  const queue = [start];
  const visited = new Set([start]);

  while (queue.length) {
    const node = queue.shift()!;
    console.log(node);

    for (const neighbor of graph[node]) {
      if (!visited.has(neighbor)) {
        visited.add(neighbor);
        queue.push(neighbor);
      }
    }
  }
}
```

> [!tip] BFS is great for finding the shortest path in **unweighted graphs**.

---

## Shortest Path Algorithms

### 1. Dijkstra’s Algorithm (for weighted graphs)

- Keeps track of the shortest known distance to each node.
- Uses a **min-priority queue** (often implemented with a heap).
- Time Complexity: `O((V + E) log V)` with a binary heap.

### 2. BFS (for unweighted graphs)

- Since every edge has equal weight, BFS gives the shortest path in `O(V + E)` time.

---

## Time & Space Complexities (Cheat Sheet)

| Structure / Operation      | Time Complexity          |
| -------------------------- | ------------------------ |
| **Binary Tree (search)**   | O(n)                     |
| **Binary Search Tree**     | O(log n) avg, O(n) worst |
| **Heap (insert/delete)**   | O(log n)                 |
| **DFS/BFS (adj list)**     | O(V + E)                 |
| **DFS/BFS (adj matrix)**   | O(V²)                    |
| **Dijkstra (binary heap)** | O((V + E) log V)         |
