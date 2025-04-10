Sorting: Bubble sort; Insertion sort; quick sort; merge; heap sort.

# Sorting Algorithms

Given a messy array, make it unmessy by _sorting_. Ascending, descending, alphabetical, IQ level..

## Quick Reference

| Algorithm      | Time (Avg) | Time (Worst) | Space    | Stable |
| -------------- | ---------- | ------------ | -------- | ------ |
| Bubble Sort    | O(n²)      | O(n²)        | O(1)     | Yes    |
| Insertion Sort | O(n²)      | O(n²)        | O(1)     | Yes    |
| Quick Sort     | O(n log n) | O(n²)        | O(log n) | No     |
| Merge Sort     | O(n log n) | O(n log n)   | O(n)     | Yes    |
| Heap Sort      | O(n log n) | O(n log n)   | O(1)     | No     |

## Bubble Sort

Repeatedly steps through the list, compares adjacent elements and swaps them if they are in the wrong order.

**Time Complexity**: O(n²)

- **Best Case**: O(n) when array is already sorted
- **Average/Worst Case**: O(n²)

**Advantages**:

- Simple to understand and implement
- Stable sorting algorithm
- Requires only O(1) extra space

**Disadvantages**:

- Very inefficient for large datasets
- Always makes O(n²) comparisons even if array is sorted

Initial array:

```mermaid
graph LR
    A1[5] --- B1[3] --- C1[8] --- D1[4] --- E1[2]
```

After first pass:

```mermaid
graph LR
    A2[3] --- B2[5] --- C2[4] --- D2[2] --- E2[8]
```

```typescript
function bubbleSort(arr: number[]): number[] {
  const n = arr.length;
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n - i - 1; j++) {
      if (arr[j] > arr[j + 1]) {
        [arr[j], arr[j + 1]] = [arr[j + 1], arr[j]];
      }
    }
  }
  return arr;
}
```

## Insertion Sort

Builds the final sorted array one item at a time, by repeatedly inserting a new element into the sorted portion of the array.

**Time Complexity**: O(n²)

- **Best Case**: O(n) when array is almost sorted
- **Average/Worst Case**: O(n²)

**Advantages**:

- Efficient for small data sets
- Adaptive: if array is nearly sorted, runs in almost O(n) time
- In-place algorithm
- Stable sort
- Online: can sort as data is being received

**Disadvantages**:

- Inefficient for large datasets
- Requires O(n²) comparisons and shifts

Initial array:

```mermaid
graph LR
    A1[5] --- B1[3] --- C1[8] --- D1[4] --- E1[2]
```

After two passes:

```mermaid
graph LR
    A2[3] --- B2[5] --- C2[8] --- D2[4] --- E2[2]
```

```typescript
function insertionSort(arr: number[]): number[] {
  for (let i = 1; i < arr.length; i++) {
    const key = arr[i];
    let j = i - 1;
    while (j >= 0 && arr[j] > key) {
      arr[j + 1] = arr[j];
      j--;
    }
    arr[j + 1] = key;
  }
  return arr;
}
```

## Quick Sort

Uses a divide-and-conquer strategy. Picks a 'pivot' element and partitions the array around it.

**Time Complexity**: O(n log n) average

- **Best Case**: O(n log n)
- **Average Case**: O(n log n)
- **Worst Case**: O(n²) when poorly pivoted

**Advantages**:

- Fastest sorting algorithm in practice
- In-place sorting
- Cache friendly
- Can be easily parallelized
- Low overhead

**Disadvantages**:

- Unstable sort
- O(n²) worst case
- Not adaptive

```mermaid
graph TD
    A[5,3,8,4,2] --> B[2,3,4 | 5 | 8]
    B --> C[2,3,4]
    B --> D[8]
    C --> E[2 | 3 | 4]
```

```typescript
function quickSort(arr: number[]): number[] {
  if (arr.length <= 1) return arr;

  const pivot = arr[Math.floor(arr.length / 2)];
  const left = arr.filter((x) => x < pivot);
  const middle = arr.filter((x) => x === pivot);
  const right = arr.filter((x) => x > pivot);

  return [...quickSort(left), ...middle, ...quickSort(right)];
}
```

## Merge Sort

Divides the array into smaller subarrays, sorts them, and then merges them back together.

**Time Complexity**: O(n log n)

- **Best/Average/Worst Case**: Always O(n log n)

**Advantages**:

- Predictable performance: always O(n log n)
- Stable sort
- Parallelizable
- Great for linked lists - no random access needed

**Disadvantages**:

- Requires O(n) extra space
- Not in-place
- Not adaptive
- Overkill for small arrays

```mermaid
graph TD
    A[5,3,8,4,2] --> B[5,3] & C[8,4,2]
    B --> D[5] & E[3]
    C --> F[8] & G[4,2]
    G --> H[4] & I[2]
```

```typescript
function mergeSort(arr: number[]): number[] {
  if (arr.length <= 1) return arr;

  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));

  return merge(left, right);
}

function merge(left: number[], right: number[]): number[] {
  const result: number[] = [];
  let i = 0,
    j = 0;

  while (i < left.length && j < right.length) {
    if (left[i] <= right[j]) {
      result.push(left[i++]);
    } else {
      result.push(right[j++]);
    }
  }

  return [...result, ...left.slice(i), ...right.slice(j)];
}
```

## Heap Sort

Uses a binary heap data structure to sort elements.

**Time Complexity**: O(n log n)

- **Best/Average/Worst Case**: Always O(n log n)

**Advantages**:

- In-place sorting
- No extra space needed
- Predictable performance
- Great for finding k largest/smallest elements

**Disadvantages**:

- Unstable sort
- Not adaptive
- Slower in practice than Quick Sort
- Poor cache performance

```mermaid
graph TD
    A((10)) --> B((4))
    A --> C((8))
    B --> D((3))
    B --> E((2))
    C --> F((6))
    C --> G((1))
```

```typescript
function heapSort(arr: number[]): number[] {
  // Build max heap
  for (let i = Math.floor(arr.length / 2) - 1; i >= 0; i--) {
    heapify(arr, arr.length, i);
  }

  // Extract elements from heap one by one
  for (let i = arr.length - 1; i > 0; i--) {
    [arr[0], arr[i]] = [arr[i], arr[0]];
    heapify(arr, i, 0);
  }

  return arr;
}

function heapify(arr: number[], n: number, i: number) {
  let largest = i;
  const left = 2 * i + 1;
  const right = 2 * i + 2;

  if (left < n && arr[left] > arr[largest]) largest = left;
  if (right < n && arr[right] > arr[largest]) largest = right;

  if (largest !== i) {
    [arr[i], arr[largest]] = [arr[largest], arr[i]];
    heapify(arr, n, largest);
  }
}
```

> [!note] Stable vs Unstable Sort
> A sorting algorithm is stable if it preserves the relative order of equal elements.
>
> - Stable: Bubble Sort, Insertion Sort, Merge Sort
> - Unstable: Quick Sort, Heap Sort
