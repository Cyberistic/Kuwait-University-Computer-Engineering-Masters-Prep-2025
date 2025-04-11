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

A symbol table, or _hash-table_, is an abstract data type that stores key-value pairs, where each key appears at most once.

In simple terms, it allows for very fast insertion and searching of items. So no matter how many data items there are, insertion and searching (and sometimes deletion) can take close to constant time _O(1)_.

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

I've never heard anyone call it a symbol table before, but here we are.

```typescript
type HashTable<K, V> = {
  [key: K]: V;
};

// Example:
const phoneBook: HashTable<string, string> = {
  sudo_kw: "+965 41000798",
  urmom: "+965 1809777"
};
```

### Disadvantages of Hash-tables:

- They're based on arrays, and arrays are difficult to expand once they are created.
- No easy way to traverse the items! Have to traverse the entire table.
- Always unsorted, expensive to do getMin, getMax, etc.

> [!Warning]
> I don't think the code implementation of hash-tables is required, but you can take a look at it here:
> [[Hash-Table & Dynamic Hash-Table implementation#Hash-Table]]

> [!Important] Fun fact..
> Want to test out how fast hash-tables are? try misspelling a word. You see that red squiggly line? it appeared instantly, but your device had to search through thousands of words saved in a hash-table.

## Hash Table Fundamentals

1. **Hash Function**

A hash function maps data of arbitrary size to fixed-size values. It is what maps key values to positions and is often donated by `h`.

A good hash function should:

    1. Be deterministic (same input = same output)
    2. Distribute values uniformly
    3. Be fast to compute
    4. Minimize collisions

Here is a simple hashing function. Assume a table with 20 slots and a hashing function of `h(k) = k % 10` and `k = sum of numeric values of letters`.

For the following strings:

- "mahdi" = 13 + 1 + 8 + 4 + 9 = 35 Hashcode = 5
- "asmaa" = 1 + 19 + 13 + 1 + 1 = 35; Hashcode = 5
- "taleb" = 20 + 1 + 12 + 5 + 2 = 40; Hashcode = 0
- "abdo" = 1 + 2 + 4 + 15 = 22; Hashcode = 2

The table would look like this:

| Index | Key   |
| ----- | ----- |
| 0     | taleb |
| 1     | empty |
| 2     | abdo  |
| 3     | empty |
| 4     | empty |
| 5     | mahdi |
| 6     | asmaa |
| 7     | empty |
| 8     | empty |
| 9     | empty |

> [!Note]
> Notice how 2 values, `mahdi` and `asmaa` had the same hash? This is called a _collision_, and the _collision resolution_ technique to use the next available slot (like how we used index 6 above) is called _Linear Probing_.
> A more detailed explanation of collisions is provided below.

2. **Hash-Table:** The _array_ that holds the records. Denoted by HT.

3. The position (Index) in a hash table is also known as a **slot**.

4. **Collisions**: Two hash tables elements map into the same slot in the array (They have the same hash).

5. **Buckets**

A bucket is a slot in the hash table that can store one or more key-value pairs:

- In open addressing: each bucket holds one item. Need to implement collision resolution function.
- In chaining: each bucket is a linked list of items. Easiest collision resolution, on collision, make the new item the tail of the linked list!

```mermaid
graph TD
    A[Hash Table] --> B[Bucket 0]
    A --> C[Bucket 1]
    A --> D[Bucket 2]
    B --> E["Entry(key1,val1)"]
    C --> F["Entry(key2,val2)"]
    C --> G["Entry(key3,val3)"]
```

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

### Disadvantages

- Fixed size might lead to poor performance if too many elements
- Waste of space if too few elements

## Dynamic Hashing

Unlike static hashing, dynamic hashing adapts its structure as elements are added/removed.

### Advantages:

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

**Advantages:**

- Simple to implement
- Performs well with good hash function
- No space waste

**Disadvantages:**

- Linked list operations overhead. (To find a node, need to traverse the entire linked-list)

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

**Advantages:**

- Better cache performance
- Simple implementation
- No extra data structures

**Disadvantages:**

- _Primary Clustering_: _Consecutive_ occupied slots form clusters
- Performance degrades as table fills up
- Deletion requires special handling

Imagine you have `n` elemts which map to the number 2

| Index | Key   |
| ----- | ----- |
| 0     | empty |
| 1     | empty |
| 2     | taleb |
| 3     | mahdi |
| 4     | asmaa |
| ...   | ...   |
| n     | abdoo |
| n+1   | empty |

What happens if you want to find "abdoo"? You have to check all the slots from 2 to n+1, which is inefficient. What if the cluster is huge? Clustering makes your fast hash table slow. riperoni my hash table.

> [!note] Once a cluster forms, it tends to grow larger. items that hash to any value in the cluster will traverse the entire cluster until an empty slot is found, AND MAKE IT LARGER!!
> The bigger the cluster gets, the faster it grows.
> Just like your mo..

### 3. Quadratic Probing

Similar to linear probing but tries slots at quadratic intervals. It is an attempt to reduce clustering.
The idea is to prove more widely separated cells, instead of adjacent ones.

It can be key + i², where `i` is the number of attempts.
For example, i + 1², i + 2², i + 3², etc.

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

**Advantages:**

- Eliminates primary clustering
- Better distribution than linear probing

**Disadvantages:**

- _Secondary clustering:_ items with same initial position probe same locations. (lol)
- May not find empty slot even when one exists
- More complex than linear probing

The disadvantages make it almost worse than linear probing!
With secondary clustering, items with the same initial position probe the same locations. So if two items hash to the same index, they will follow the same probing sequence.

Another disadvantage is that due to how it "skips" over slots, it may not find an empty slot ever!
For example, lets say you have a table of size 4:

| Index | Key   |
| ----- | ----- |
| 0     | asmaa |
| 1     | mahdi |
| 2     | taleb |
| 3     | empty |

Now let's say you want to insert "abdo" which hashes to 0. You'll try 1, then 4 % 4 = 0, then 9 % 4 = 1, then 16 % 4 = 0, then 25 % 4 = 1, and so on. You will never find an empty slot, even though there is one at index 3.

This is fixable by using prime numbers for the table size! the infinite loop does not happen if the table size is prime. All slots will be probed eventually.

### 4. Double Hashing

Uses a second hash function to determine the probe interval.
When hashing item x, if the first hash function returns index i, resolve collision by putting the colliding element into slot i + h2(x), i + 2 _ h2(x), i + 3 _ h2(x), etc.

nerdy experts have determined that the best function is step = constant - (key % constant), where constant is a prime number less than the table size.

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

## Load Factor

The load factor is the ratio of the number of elements in the hash table to the size of the table. It is a measure of how full the hash table is.
A load factor of 0.7 is often considered optimal for performance. If the load factor exceeds a certain threshold (e.g., 0.7), the hash table may need to be resized.
This is because as the load factor increases, the probability of collisions increases, leading to slower performance.

In chaining, the load factor can be greater than 1, as multiple elements can be stored in the same bucket. Meaning, you can have more elements than number of slots in the table. 🤯
