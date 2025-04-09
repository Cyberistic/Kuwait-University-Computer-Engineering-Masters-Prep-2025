Singly linked lists; circularly linked lists; applications - polynomial addition, sparse matrices; doubly linked lists, and complexities of linked list operations.


## Singly Linked Lists
![[Singly Linked List.png]]

A *Singly Linked Lists* is a list of nodes, each node has 2 properties:
1. Stored value (Can be a pointer to an object)
2. Reference to next node (its "address" in memory)

We only need to track the address of the Head, and then we can then find any node by traversing the list by *pointer hopping*.

We can identify the tail (last node) of the last if its next value is `None`. I usually like to have a reference to the end value as well, such that adding elements to the linked list doesn't require traversing the entire list.

So in general, this is how it would look like:

```ts
type Node = {
  id: string; // (optional) if you want to search by id
  value: number;
  next: Node | null; // if null then it's tail
  count: number; // Always nice to have the total number of nodes
};

type LinkedList = {
  head: Node | null;
  tail: Node | null;
  // + Rest of functions like add, etc.
};

```

So in this implementation, adding or removing nodes should be straightforward: 

1. Add at start (Head): 
```ts
// Complexity: O(1)
let newNode = new Node();
newNode.next = linkedlist.head;
linkedlist.head = newNodel
```

2. Add at end (tail):
```ts
// Case 1:
// Complexity: O(1)
let newNode = new Node();
newNode.next = linkedlist.head;
linkedlist.head = newNodel
```