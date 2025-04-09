Singly linked lists; circularly linked lists; applications - polynomial addition, sparse matrices; doubly linked lists, and complexities of linked list operations.

> [!info] Note
> This requires understanding of Time Complexity. For a refresher, check out [[Time Complexities & Recursion]].

## Singly Linked Lists

![[Singly Linked List.png]]

A _Singly Linked Lists_ is a list of nodes, each node has 2 properties:

1. Stored value (Can be a pointer to an object)
2. Reference to next node (its "address" in memory)

We only need to track the address of the Head, and then we can then find any node by traversing the list by _pointer hopping_.

We can identify the tail (last node) of the last if its next value is `None`. I usually like to have a reference to the end value as well, such that adding elements to the linked list doesn't require traversing the entire list.

So in general, this is how it would look like:

```ts
type Node = {
  id: string; // (optional) if you want to search by id
  value: number;
  next: Node | null; // if null then it's tail
};

type LinkedList = {
  head: Node | null;
  tail: Node | null;
  count: number; // Always nice to have the total number of nodes
  // + Rest of functions like add, etc.
};
```

So in this implementation, adding or removing nodes should be straightforward:

1. Add at start (Head):

```ts
// Complexity: O(1)
function addToHead(linkedList: LinkedList, newNode: Node) {
  newNode.next = linkedList.head;
  linkedList.head = newNode;
  linkedList.count++;
}
```

2. Add at end (tail):

```ts
// Case 1: We are keeping track of tail
// Complexity: O(1)
function addToTail(linkedList: LinkedList, newNode: Node) {
  linkedList.tail.next = newNode;
  linkedList.tail = newNode;
  linkedList.count++;
}

// Case 2: Not keeping track of tail
function addToTail(linkedList: LinkedList, newNode: Node) {
  let current = linkedList.head;
  while (current.next) {
    current = current.next;
  }
  current.next = newNode;
  linkedList.count++;
}
```

3. Add in the middle:

```ts
// let's say we want to search for id and add after it
// Complexity: O(n)
function addAfterId(linkedList: LinkedList, id: string, newNode: Node) {
  let current = linkedList.head;
  while (current) {
    if (current.id === id) {
      break;
    }
    current = current.next;
  }
  newNode.next = current.next;
  current.next = newNode;
}
```

Removing nodes is a bit more tricky, because we need to keep track of the previous node as well. So we can do something like this:

```ts
// Complexity: O(n)
function removeById(linkedList: LinkedList, id: string) {
  if (linkedList.head === null) {
    return;
  }
  linkedList.count--;
  // if we are keeping track of tail, we need to check if we are removing the last node
  if (linkedList.tail === linkedList.head) {
    linkedList.tail = null;
  }
  // if we are removing the head
  if (linkedList.head.id === id) {
    linkedList.head = linkedList.head.next;
    return;
  }
  // if we are removing a middle or tail node
  let current = linkedList.head;
  let previous = null;
  while (current) {
    if (current.id === id) {
      break;
    }
    previous = current;
    current = current.next;
  }
  previous.next = current.next;
  current = null; // or use a garbage collector, or implement a destructor, or just let it live in memory forever lol
}
```

Same thing with other remove operations, just do the addition operation "backwards" so to speak.
So for example, removing the head would look like this:

```ts
// Complexity: O(1)
function removeHead(linkedList: LinkedList) {
  if (linkedList.head === null) {
    return;
  }
  linkedList.count--; // if we are keeping track of tail, we need to check if we are removing the last node
  if (linkedList.tail === linkedList.head) {
    linkedList.tail = null;
  }
  linkedList.head = linkedList.head.next;
}
```

### Software Applications

#### Stack

A stack is a data structure that follows the _Last In First Out (LIFO)_ principle. It can be implemented using a singly linked list. The top of the stack corresponds to the head of the linked list, and we can add or remove elements from the top of the stack in constant time _O(1)_.

```ts
// to push to stack
function push(linkedList: LinkedList, newNode: Node) {
  addToHead(linkedList, newNode);
}
// to pop from stack
function pop(linkedList: LinkedList) {
  if (linkedList.head === null) {
    return null;
  }
  const poppedNode = linkedList.head;
  removeHead(linkedList);
  return poppedNode;
}
```

I added full code for [[Implementing A Stack using Python]], take a quick look at it.

#### Queue

A queue is a data structure that follows the _First In First Out (FIFO)_ principle. It can be implemented using a singly linked list. The front of the queue corresponds to the head of the linked list, and we can add elements to the end of the queue in constant time O(1).

```ts
// to enqueue
function enqueue(linkedList: LinkedList, newNode: Node) {
  addToTail(linkedList, newNode);
}
// to dequeue
function dequeue(linkedList: LinkedList) {
  if (linkedList.head === null) {
    return null;
  }
  const dequeuedNode = linkedList.head;
  removeHead(linkedList);
  return dequeuedNode;
}
```

> [!tip] Fun fact
> A fun application of a queue is the "Round Robin" scheduler, where you enqueue a dequeue'd process in a sort of loop.

### Math Applications

#### Polynomial Addition

Polynomials can be efficiently represented using linked lists, where each node represents a term with its coefficient and exponent.

A polynomial $P(x) = a_nx^n + a_{n-1}x^{n-1} + \ldots + a_1x + a_0$ can be represented as a linked list where each node contains:

- Coefficient $a_i$
- Exponent $i$
- Pointer to the next term

For example, the polynomial $3x^4 + 2x^2 + 5x + 7$ would be represented as:

```ts
type PolynomialNode = {
  coefficient: number;
  exponent: number;
  next: PolynomialNode | null;
};
type Polynomial = {
  head: PolynomialNode | null;
  tail: PolynomialNode | null;
  count: number; // Always nice to have the total number of nodes
};
```

## Circular Linked List

![[Circularly-Linked-List.png]]

Instead of keeping track of head and tail, the last element points to the first, forming a loop.

All operations are O(n) since we are not keeping track of any head or tail. Adding and removing is usually done with id.

```ts
type LinkedList = {
  current: Node | null; // Any Node to start with
  count: number; // Always nice to have the total number of nodes
  // + Rest of functions like add, etc.
};
```

> [!tip] Fun fact
> If a list has a single node, it literally points to itself.
> ![[Single-Node-Circular-List.png|100]]

Here is a fun challenge, implement the _Round Robin Scheduler_ with a Circular linked list.

## Doubly Linked List

![[Doubly Linked Lists.png]]
A doubly linked list is a linked list where each node has a reference to both the next and previous nodes. This allows for more efficient traversal in both directions.

```ts
type Node = {
  id: string; // (optional) if you want to search by id
  value: number;
  next: Node | null; // if null then it's tail
  prev: Node | null; // if null then it's head
};
type LinkedList = {
  head: Node | null;
  tail: Node | null;
  count: number; // Always nice to have the total number of nodes
  // + Rest of functions like add, etc.
};
```

### Adding and Removing Nodes

Adding and removing nodes is similar to a singly linked list, but we need to keep track of the previous node as well. So we can do something like this:

1. Add at start (Head):

```ts
// Complexity: O(1)
function addToHead(linkedList: LinkedList, newNode: Node) {
  newNode.next = linkedList.head;
  if (linkedList.head) {
    linkedList.head.prev = newNode;
  }
  linkedList.head = newNode;
  linkedList.count++;
}
```

2. Add at end (tail):

```ts
// Complexity: O(1)
function addToTail(linkedList: LinkedList, newNode: Node) {
  if (linkedList.tail) {
    linkedList.tail.next = newNode;
    newNode.prev = linkedList.tail;
  }
  linkedList.tail = newNode;
  linkedList.count++;
}
```

3. Add in the middle:

```ts
// Complexity: O(n)
// Much nicer than singly linked list, this is where it shines
function addAfterId(linkedList: LinkedList, id: string, newNode: Node) {
  let current = linkedList.head;
  while (current) {
    if (current.id === id) {
      break;
    }
    current = current.next;
  }
  newNode.next = current.next;
  newNode.prev = current;
  if (current.next) {
    current.next.prev = newNode;
  }
  current.next = newNode;
}
```

4. Removing

```ts
// Complexity: O(n)
// much nicer than singly linked list
function removeById(linkedList: LinkedList, id: string) {
  if (linkedList.head === null) {
    return;
  }
  linkedList.count--;
  // if we are keeping track of tail, we need to check if we are removing the last node
  if (linkedList.tail === linkedList.head) {
    linkedList.tail = null;
  }
  // if we are removing the head
  if (linkedList.head.id === id) {
    linkedList.head = linkedList.head.next;
    linkedList.head.prev = null;
    return;
  }
  // if we are removing a middle or tail node
  let current = linkedList.head;
  while (current) {
    if (current.id === id) {
      break;
    }
    current = current.next;
  }
  current.prev.next = current.next;
  if (current.next) {
    current.next.prev = current.prev;
  }
}
```

Everything else is almost the same as singly linked lists.
