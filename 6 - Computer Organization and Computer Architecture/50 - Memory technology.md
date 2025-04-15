# Memory Technology and Cache Design

## Memory Hierarchy

```mermaid
graph TD
    A[Registers] --> B[Cache]
    B --> C[Main Memory]
    C --> D[Virtual Memory]
    D --> E[Secondary Storage]
```

## Cache Memory

### Basic Concepts

- Smaller, faster memory between CPU and main memory
- Holds frequently accessed data
- Managed by hardware

### Cache Organization

1. **Direct Mapped**: Each memory location maps to exactly one cache location

   ```
   Cache Location = Memory Address MOD Cache Size
   ```

2. **Set Associative**: Each memory location can go in n locations
3. **Fully Associative**: Memory location can go anywhere

### Hit Rate Calculations

```
Hit Rate = Hits / (Hits + Misses)
Average Access Time = Hit Time + (Miss Rate × Miss Penalty)
```

Example:

```
Hit Rate = 0.8 (80%)
Hit Time = 1 cycle
Miss Penalty = 10 cycles

Average Access Time = 1 + (0.2 × 10) = 3 cycles
```

## Virtual Memory

### Page Size Calculations

```
Page Size = 2^n bytes (where n is number of offset bits)
Number of Pages = Total Memory Size / Page Size
```

Example:

```
Memory Size = 4GB = 2^32 bytes
Page Size = 4KB = 2^12 bytes
Number of Pages = 2^32 / 2^12 = 2^20 pages
```

### Address Translation

Physical Address = (Page Number × Page Size) + Offset

For a 32-bit address with 4KB pages:

- Offset bits = 12 (2^12 = 4096 bytes)
- Page number bits = 20 (32 - 12)

### Page Table Entry (PTE)

```
|Valid Bit|Modified Bit|Referenced Bit|Protection|Page Frame Number|
|   1    |     1     |      1      |    3     |       20       | bits
```

### TLB (Translation Lookaside Buffer)

- Cache for page table entries
- Contains most recently used translations

```mermaid
graph LR
    A[Virtual Address] --> B[TLB]
    B -->|Hit| D[Physical Address]
    B -->|Miss| C[Page Table]
    C --> D
```

## Memory Performance

### Access Time Calculation

```
Effective Access Time =
    (Hit Rate × Hit Time) +
    (Miss Rate × (Hit Time + Memory Access Time))
```

Example:

```
Hit Rate = 0.95
Hit Time = 1ns
Memory Access = 100ns

Effective Time = (0.95 × 1) + (0.05 × (1 + 100))
                = 0.95 + 5.05
                = 6ns
```

### Memory Bandwidth

```
Bandwidth = Bus Width × Clock Frequency
```

Example:

```
64-bit bus at 200MHz = 64 bits × 200,000,000 = 12.8 Gbps
```

## Common Calculations

1. **Cache Size**

```
Total Cache Size =
    Number of Lines ×
    (Line Size + Valid Bit + Tag Bits)
```

2. **Tag Bits**

```
Tag Bits =
    Address Bits -
    (Log2(Number of Lines) + Log2(Line Size))
```

3. **Page Table Size**

```
Page Table Size =
    Number of Pages ×
    Size of Page Table Entry
```

Example:

```
4GB address space
4KB pages
4 bytes per PTE

Number of Pages = 2^32/2^12 = 2^20
Page Table Size = 2^20 × 4 = 4MB
```

> [!tip] Memory Design Trade-offs
>
> 1. Larger page size:
>    - Less page table entries (+)
>    - More internal fragmentation (-)
> 2. Larger cache line size:
>    - Better spatial locality (+)
>    - Higher miss penalty (-)
> 3. More cache associativity:
>    - Lower miss rate (+)
>    - More complex hardware (-)
