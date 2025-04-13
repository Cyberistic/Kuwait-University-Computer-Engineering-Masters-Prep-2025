This note's job is to display the time complexities for all the algorithms required in the Data Structures material.
______________
### 1. Linked lists:

| Operation              | Singly Linked List | Circular Linked List | Doubly Linked List |
| ---------------------- | ------------------ | -------------------- | ------------------ |
| **Traversal**              | O(n)               | O(n)                 | O(n)               |
| **Insertion at Beginning** | O(1)               | O(1)                 | O(1)               |
| **Insertion at End**       | O(n)               | O(1)\*               | O(1)\*             |
| **Insertion at Middle**    | O(n)               | O(n)                 | O(n)               |
| **Deletion at Beginning**  | O(1)               | O(1)                 | O(1)               |
| **Deletion at End**        | O(n)               | O(n)                 | O(1)               |
| **Deletion at Middle**     | O(n)               | O(n)                 | O(n)               |
| **Search**                 | O(n)               | O(n)                 | O(n)               |
| **Polynomial Addition**    | O(n + m)           | O(n + m)             | O(n + m)           |
_________

### 2. Trees and graphs:

| Operation / Structure                   | Time Complexity                         |
|----------------------------------------|------------------------------------------|
| **Binary Tree Traversals**             |                                          |
| - Inorder / Preorder / Postorder       | O(n)                                     |
|                                        |                                          |
| **Binary Search Tree (BST)**           |                                          |
| - Search / Insert / Delete (average)   | O(log n)                                 |
| - Search / Insert / Delete (worst)     | O(n) (if unbalanced)                     |
|                                        |                                          |
| **Balanced BST (AVL, Red-Black)**      |                                          |
| - Search / Insert / Delete             | O(log n)                                 |
|                                        |                                          |
| **Heaps (Min/Max)**                    |                                          |
| - Insert                               | O(log n)                                 |
| - Delete (Max or Min)                  | O(log n)                                 |
| - Find Max/Min                         | O(1)                                     |
|                                        |                                          |
| **Tree Representation of General Trees**|                                          |
| - Using Binary Tree Representation     | O(n) (traversal)                         |
|                                        |                                          |
| **Graph Representations**              |                                          |
| - Adjacency Matrix                     | O(1) edge check, O(V²) space             |
| - Adjacency List                       | O(V + E) traversal, O(V + E) space       |
|                                        |                                          |
| **Graph Traversals**                   |                                          |
| - DFS / BFS                            | O(V + E)                                 |
|                                        |                                          |
| **Shortest Path**                      |                                          |
| - Dijkstra (with Min Heap)             | O((V + E) log V)                         |
| - Bellman-Ford                         | O(VE)                                    |
| - Floyd-Warshall (All-pairs)           | O(V³)                                    |
___________

### 3. Sorting:
| Sorting Algorithm | Best Case  | Average Case | Worst Case | Space Complexity | Stable |
| ----------------- | ---------- | ------------ | ---------- | ---------------- | ------ |
| Bubble Sort       | O(n)       | O(n²)        | O(n²)      | O(1)             | Yes    |
| Insertion Sort    | O(n)       | O(n²)        | O(n²)      | O(1)             | Yes    |
| Quick Sort        | O(n log n) | O(n log n)   | O(n²)      | O(log n)         | No     |
| Merge Sort        | O(n log n) | O(n log n)   | O(n log n) | O(n)             | Yes    |
| Heap Sort         | O(n log n) | O(n log n)   | O(n log n) | O(1)             | No     |
> Stable? if two elements have **equal keys**, their **relative order stays the same** after sorting.