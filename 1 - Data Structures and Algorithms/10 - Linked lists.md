Singly linked lists; circularly linked lists; applications - polynomial addition, sparse matrices; doubly linked lists, and complexities of linked list operations.


## Singly Linked Lists
![[Singly Linked List.png]]

A *Singly Linked Lists* is a list of nodes, each node has 2 properties:
1. Stored value (Can be a pointer to an object)
2. Pointer to next node (its "address" in memory)

We only need to track the address of the Head, and then we can find the rest of the 
We can identify the tail (last node) of the last if its next value is `None`. I usually like to have a reference to the end value as well, such that adding elements to the linked list doesn't require traversing the entire list.

So in general, this is how it would look like:
```ts
type Node = {
  id
  value: number;
  next: Node | null;
};

type LinkedList = {
  head: Node | null;
  tail: Node | null;
  
};

```