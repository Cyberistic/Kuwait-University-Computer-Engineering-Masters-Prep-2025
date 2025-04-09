
### Question:

**Which of the following binary trees correctly represents the general tree using the Left-Child Right-Sibling technique?**

#### General Tree:

```
       A
     / | \
    B  C  D
   /     / \
  E     F   G
```

### Choices:

**A)**

```
       A
     / 
    B  
   / \  
  E   C
     / 
    D
   / \
  F   G
```

**B)**

```
       A
     / 
    B  
     \
      C
     / 
    D
   / \
  F   G
```

**C)**

```
       A
     / \
    B   C
   /   / \
  E   D   G
     / \
    F   H
```

**D)**

```
       A
     / 
    B  
   /   
  E    C
       / \
      D   G
```

---

### Answer Key:

The correct answer is **A**.

### Explanation:

- **Option A** correctly follows the Left-Child Right-Sibling technique, where:
    
    - **A** has **B** as its left child and **C** as its right child.
        
    - **B** has **E** as its left child and no right sibling.
        
    - **C** has **D** as its left child and no right sibling.
        
    - **D** has **F** as its left child and **G** as its right sibling.