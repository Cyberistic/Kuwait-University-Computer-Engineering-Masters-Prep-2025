Hashing: Symbol tables; static hashing; dynamic hashing, and collision resolution techniques.

## Hashing

Hashing is a technique used to map data of arbitrary size to fixed-size values. Think of it as a way to create an "index" for your data.

This is super useful for fast data retrieval, like when you want to find a specific item in a large dataset.
Hashing is used in various applications, including:

- **Databases**: For indexing and quick lookups.
- **Cryptography**: For data integrity and security. (e.g., hashing passwords).
- **Caching**: To store frequently accessed data.
- **Data Structures**: Like hash tables, which allow for efficient data retrieval.

## Symbol Tables (Hash Tables)

A symbol table is an abstract data type that stores key-value pairs, where each key appears at most once.

We use this all the time, think dictionaries in javascript, or a map in python.

```ts
const IQ = {
  Mahdi: 60,
  Asmaa: 130,
  Taleb: 130,
  Abdo: 90
};
```

Ever noticed how fast it is to access the IQ of "Mahdi" using `IQ["Mahdi"]`? That's the power of hash tables.

> [!Question]- We even use nested tables all the time, what do you think `JSON` is?
> It's a nested hash table.
>
> ```ts
> const IQ = {
>   Mahdi: {
>     IQ: 60,
>     age: 25
>   },
>   Asmaa: {
>     IQ: 130,
>     age: 8
>   },
>   Taleb: {
>     IQ: 130,
>     age: 13
>   },
>   Abdo: {
>     IQ: 90,
>     age: 49
>   }
> };
> ```
>
> I've never heard anyone call it a symbol table before, but here we are.

```typescript
type HashTable<K, V> = {
  [key: K]: V;
};

// Example:
const phoneBook: HashTable<string, string> = {
  John: "1809777",
  urmom: "098-765-4321"
};
```

> [!Important]
> I don't think the code implementation of hash-tables is required, but you can take a look at it here:
> [[Hash-Table & Dynamic Hash-Table implementation#Hash-Table]]

## Static Hashing

In static hashing, the hash table size remains constant and is determined at creation time.

```mermaid
graph TD
    A[Key] --> B[Hash Function]
    B --> C[Fixed-size Table]
    C --> D[Bucket 0]
    C --> E[Bucket 1]
    C --> F[...]
    C --> G[Bucket n-1]
```

### Advantages

- Simple implementation
- Constant-time operations O(1) (average case)

### Disadvantages

- Fixed size might lead to poor performance if too many elements
- Waste of space if too few elements

## Hash Table Fundamentals

### Hash Function

A hash function maps data of arbitrary size to fixed-size values. A good hash function should:

1. Be deterministic (same input = same output)
2. Distribute values uniformly
3. Be fast to compute
4. Minimize collisions

Example of a simple hash function:

```typescript
function hash(key: string): number {
  let hash = 0;
  for (let i = 0; i < key.length; i++) {
    hash = (hash << 5) - hash + key.charCodeAt(i);
    hash = hash & hash; // Convert to 32-bit integer
  }
  return Math.abs(hash);
}
```

### Buckets

A bucket is a slot in the hash table that can store one or more key-value pairs:

- In open addressing: each bucket holds one item
- In chaining: each bucket is a linked list of items

```mermaid
graph TD
    A[Hash Table] --> B[Bucket 0]
    A --> C[Bucket 1]
    A --> D[Bucket 2]
    B --> E["Entry(key1,val1)"]
    C --> F["Entry(key2,val2)"]
    C --> G["Entry(key3,val3)"]
```

## Dynamic Hashing

Unlike static hashing, dynamic hashing adapts its structure as elements are added/removed.

### Load Factor Based Resizing

```typescript
class DynamicHashTable<K, V> {
  private table: Array<Array<[K, V]>>;
  private size: number = 0;
  private capacity: number;
  private loadFactorThreshold = 0.75;

  private resize(newCapacity: number): void {
    const oldTable = this.table;
    this.table = new Array(newCapacity).fill(null).map(() => []);
    this.capacity = newCapacity;
    this.size = 0;

    // Rehash all existing entries
    for (const bucket of oldTable) {
      for (const [key, value] of bucket) {
        this.set(key, value);
      }
    }
  }

  set(key: K, value: V): void {
    if (this.size / this.capacity >= this.loadFactorThreshold) {
      this.resize(this.capacity * 2);
    }
    // ...rest of set implementation
  }
}
```

### Advantages of Dynamic Hashing

- Better space utilization
- Maintains good performance as data grows
- Adapts to workload

### Disadvantages

- Resizing operation is expensive (O(n))
- More complex implementation
- Memory usage can spike during resizing

## Collision Resolution

When two keys hash to the same index, we have a collision. Here are common resolution techniques:

### 1. Chaining (Using Linked Lists)

```mermaid
graph TD
    A[Hash Table] --> B[0]
    A --> C[1]
    A --> D[2]
    B --> E[Key1 -> Val1]
    E --> F[Key2 -> Val2]
    C --> G[Key3 -> Val3]
```

```typescript
class ChainedHashTable<K, V> {
  table: Array<Array<[K, V]>>;

  set(key: K, value: V): void {
    const index = this.hash(key);
    // Simply append to the chain
    this.table[index].push([key, value]);
  }
}
```

### 2. Linear Probing

If a collision occurs, try the next slot until an empty one is found.

```typescript
class LinearProbingHashTable<K, V> {
  table: Array<[K, V] | null>;

  set(key: K, value: V): void {
    let index = this.hash(key);

    while (this.table[index] !== null) {
      index = (index + 1) % this.size;
    }

    this.table[index] = [key, value];
  }
}
```

### 3. Quadratic Probing

Similar to linear probing but tries slots at quadratic intervals.

```typescript
class QuadraticProbingHashTable<K, V> {
  set(key: K, value: V): void {
    let index = this.hash(key);
    let i = 0;

    while (this.table[index] !== null) {
      i++;
      index = (this.hash(key) + i * i) % this.size;
    }

    this.table[index] = [key, value];
  }
}
```

### 4. Double Hashing

Uses a second hash function to determine the probe interval.

```typescript
class DoubleHashTable<K, V> {
  private hash2(key: K): number {
    // Second hash function
    return 7 - (this.hash(key) % 7);
  }

  set(key: K, value: V): void {
    let index = this.hash(key);
    const step = this.hash2(key);

    while (this.table[index] !== null) {
      index = (index + step) % this.size;
    }

    this.table[index] = [key, value];
  }
}
```

## Performance Comparison

| Method            | Average Case | Worst Case | Space    |
| ----------------- | ------------ | ---------- | -------- |
| Chaining          | O(1 + α)     | O(n)       | O(n + m) |
| Linear Probing    | O(1/(1-α))   | O(n)       | O(n)     |
| Quadratic Probing | O(1/(1-α))   | O(n)       | O(n)     |
| Double Hashing    | O(1/(1-α))   | O(n)       | O(n)     |

Where:

- α = load factor (n/m)
- n = number of elements
- m = table size

> [!note] Load Factor
> The load factor α = n/m determines how full the hash table is. A higher load factor means more collisions but better space utilization. Most implementations keep α < 0.75.

### Problems with Different Resolution Methods:

#### 1. Chaining

**Advantages:**

- Simple to implement
- Performs well with good hash function
- No space waste

**Disadvantages:**

- Extra memory for linked list nodes
- Poor cache performance
- Linked list operations overhead

#### 2. Linear Probing

**Advantages:**

- Better cache performance
- Simple implementation
- No extra data structures

**Disadvantages:**

- Primary clustering: consecutive occupied slots form clusters
- Performance degrades as table fills up
- Deletion requires special handling

```mermaid
graph LR
    A["Index 5"] --> B["Index 6"] --> C["Index 7"] --> D["Index 8"]
    style A fill:#f9f,stroke:#333
    style B fill:#f9f,stroke:#333
    style C fill:#f9f,stroke:#333
    style D fill:#f9f,stroke:#333
```

#### 3. Quadratic Probing

**Advantages:**

- Eliminates primary clustering
- Better distribution than linear probing

**Disadvantages:**

- Secondary clustering: items with same initial position probe same locations
- May not find empty slot even when one exists
- More complex than linear probing

#### 4. Double Hashing

**Advantages:**

- No clustering
- Better distribution than linear/quadratic probing

**Disadvantages:**

- Requires second hash function
- More expensive computation
- Complex implementation

> [!tip] Choosing a Resolution Method
>
> - Use **Chaining** when:
>   - Load factor might exceed 0.7
>   - Delete operations are frequent
> - Use **Linear Probing** when:
>   - Cache performance is critical
>   - Load factor stays below 0.7
> - Use **Double Hashing** when:
>   - Maximum performance is needed
>   - Space is at a premium
