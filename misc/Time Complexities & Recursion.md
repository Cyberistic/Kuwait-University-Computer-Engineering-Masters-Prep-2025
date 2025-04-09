# Time Complexities

Time complexity is a way to express how the runtime of an algorithm grows as the size of the input increases.

## Visual Comparison of Time Complexities

```mermaid
%%{init: {'theme': 'base', 'themeVariables': { xychart-beta: {'background': '#ffffff' }}}}%%
xychart-beta
    title "Growth of Different Time Complexities"
    x-axis [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    y-axis "Operations" 0 --> 100
    line [1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
    line [0, 1, 1, 2, 2, 2, 3, 3, 3, 3, 3]
    line [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    line [0, 1, 2, 5, 8, 12, 17, 21, 26, 31, 36]
    line [0, 1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
    line [0, 1, 8, 27, 64, 100, 100, 100, 100, 100, 100]
```

**Chart Legend:**

- blue: O(1) - Constant Time
- green: O(log n) - Logarithmic Time
- yellow: O(n) - Linear Time
- orange: O(n log n) - Linearithmic Time
- red: O(n²) - Quadratic Time
- purple: O(n³) - Cubic Time

> [!note] Note on the chart
> The values for O(2ⁿ) and O(n!) grow too quickly to display on the same chart (at n=10, O(2ⁿ) = 1024 and O(n!) = 3,628,800), so they've been omitted from the visual representation.

## Common Time Complexities (From Best to Worst)

### O(1) - Constant Time

- **Description**: Execution time remains constant regardless of input size.
- **Examples**:
  - Array access by index
  - Hash table insertion/lookup (average case)
  - Stack operations (push/pop)

### O(log n) - Logarithmic Time

- **Description**: Execution time grows logarithmically with input size.
- **Examples**:
  - Binary search
  - Operations on balanced binary search trees
  - Divide and conquer algorithms

### O(n) - Linear Time

- **Description**: Execution time grows linearly with input size.
- **Examples**:
  - Linear search
  - Traversing arrays or linked lists
  - Finding min/max in an unsorted array

### O(n log n) - Linearithmic Time

- **Description**: Combination of linear and logarithmic growth.
- **Examples**:
  - Efficient sorting algorithms (Merge sort, Heap sort, Quick sort average case)
  - Many divide and conquer algorithms

### O(n²) - Quadratic Time

- **Description**: Execution time grows with the square of the input size.
- **Examples**:
  - Bubble sort, Insertion sort, Selection sort
  - Nested loops iterating over the same collection
  - Comparing all pairs of elements in an array

### O(n³) - Cubic Time

- **Description**: Execution time grows with the cube of the input size.
- **Examples**:
  - Some inefficient matrix operations
  - Floyd-Warshall algorithm for finding shortest paths in a graph
  - Some dynamic programming solutions

### O(2ⁿ) - Exponential Time

- **Description**: Execution time doubles with each additional input element.
- **Examples**:
  - Recursive Fibonacci calculation
  - Power set calculation
  - Brute force solutions to the Traveling Salesman Problem

### O(n!) - Factorial Time

- **Description**: Execution time grows with the factorial of the input size.
- **Examples**:
  - Generating all permutations of a set
  - Brute force solution to the Traveling Salesman Problem
  - Deterministically solving NP-complete problems

## Tips for Algorithm Optimization

1. **Reduce Constants**: Even if they don't affect asymptotic complexity, constants matter in practice.
2. **Choose Appropriate Data Structures**: Using the right data structure can significantly improve performance.
3. **Avoid Nested Loops**: When possible, find a way to solve the problem without nesting multiple loops.
4. **Memoization**: Cache previously computed results to avoid redundant calculations.
5. **Use Efficient Algorithms**: Replace inefficient algorithms with more efficient ones when possible.

> [!note] Memory Complexity
> Don't forget that algorithms also have space complexity, which measures the amount of memory required as input size grows. Space-time tradeoffs are often important considerations in algorithm design.
