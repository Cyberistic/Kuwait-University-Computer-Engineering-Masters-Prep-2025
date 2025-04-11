## Hash-Table
```typescript
class HashTable<K, V> {
  private table: Array<Array<[K, V]>>;
  private size: number;

  constructor(size: number = 16) {
    this.table = new Array(size).fill(null).map(() => []);
    this.size = size;
  }

  private hash(key: K): number {
    const str = String(key);
    let hash = 0;
    for (let i = 0; i < str.length; i++) {
      hash = (hash << 5) - hash + str.charCodeAt(i);
      hash = hash & hash; // Convert to 32-bit integer
    }
    return Math.abs(hash % this.size);
  }

  set(key: K, value: V): void {
    const index = this.hash(key);
    const bucket = this.table[index];

    // Check if key already exists
    for (let i = 0; i < bucket.length; i++) {
      if (bucket[i][0] === key) {
        bucket[i][1] = value;
        return;
      }
    }

    // Add new key-value pair
    bucket.push([key, value]);
  }

  get(key: K): V | undefined {
    const index = this.hash(key);
    const bucket = this.table[index];

    for (const [k, v] of bucket) {
      if (k === key) return v;
    }

    return undefined;
  }
}

```


## Dynamic Hash-Table
```typescript
class DynamicHashTable<K, V> extends HashTable<K, V> {
  private count: number = 0;
  private loadFactorThreshold: number = 0.75;

  private resize(newSize: number): void {
    const oldTable = this.table;
    this.table = new Array(newSize).fill(null).map(() => []);
    this.size = newSize;

    // Rehash all existing entries
    for (const bucket of oldTable) {
      for (const [key, value] of bucket) {
        this.set(key, value);
      }
    }
  }

  set(key: K, value: V): void {
    super.set(key, value);
    this.count++;

    if (this.count / this.size > this.loadFactorThreshold) {
      this.resize(this.size * 2);
    }
  }
}
```