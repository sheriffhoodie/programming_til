## Trees and Graphs
- a tree is a data structure of nodes
- each tree has a root node, the root node has zero or more child nodes and each child node has zero or more child nodes
- binary tree - when each node has up to two children
- n-ary tree - when each node has up to n children
- a “leaf” node has no children
- binary search tree - a binary tree in which every node fits a specific ordering property, e.g. all left children <= n <= all right children
- balanced - not completely perfect, but balanced enough to ensure O(log n) time for insertion and finding. types of balanced trees:
- complete binary trees - every level of the tree up to the bottom is filled, from left to right
- full binary trees - every node has either zero or two children, ie no nodes have only one child
- perfect binary trees - both full and complete, all levels complete with maximum number of children. A perfect binary tree has exactly 2^k - 1 nodes, where k is the number of levels

### AVL Trees
- stores in each node the height of the subtrees rooted at this node, then for any node, you can check if it is height-balanced: the height of the left subtree and the height of the right subtree differ by no more than one
- insertion of a new node will cause the height balance to change, this is fixed with either left or right rotations
- height of a node is the length of the longest path from that node to a leaf
- for height violations, start with lowest subtree violation
- for an LL or RR rotation, the middle node becomes the new root node, the top node becomes the right child, and the bottom becomes the left child
- for an LR or RL rotation, the bottom node becomes the new root node, the middle node becomes the right child, and the top node becomes the left child
- repeat this process moving up the tree until all height balance violations are fixed

### Red-Black trees
- a type of self-balancing binary search tree, balanced enough for O(log n) insertion, deletion, and retrieval
- compared to AVL trees, they require less memory and can rebalance faster so they’re often used in situations where the tree will be modified frequently
- follow certain property rules and require every path from a node to its leaves have the same number of black nodes
- Rules:
  - every node is either red or black
  - the root is black
  - the leaves, or null nodes, are black
  - every red node must have two black children, ie red nodes can’t have red children but black nodes can have black children. This means that no more than half the nodes in a path can be red
every path from a node to its leaves must have the same number of black children
- all of this means that the lengths of paths cannot differ by more than a factor of 2, which guarantees the runtime
- insertion:
  - new nodes are inserted at a leaf which means that they replace a black node
  - new nodes are always red and are given two black leaf (null) nodes
  - now need to fix any red violations that appear (a red node has a red child)
  - given a N current node, P parent node, U uncle node, and G grandparent node:
    - P and N are both red (the violation), which means G is black
    - if U is red, can simply switch the colors of P, U and G. If this creates a red violation with G’s parents, recursively apply same idea with G now set as N

### Tries (Prefix Trees) -
- variant of an n-ary tree in which characters are stored at each node and each path down the tree may represent a word
- the * (null) nodes / leaves of the trie are often used to indicate complete words, they may be designated as a special subclass of a child node, or there may be a terminating boolean in the parent node
- a node in a trie could have anywhere from 0 to alphabet_size  + 1 children
- commonly, a trie is used to store the entire English language for quick prefix lookups
- a trie can check if a string is a valid prefix in O(K) time, where K is the length of the string

### Graphs
- a graph is simply a collection of nodes with a number of connections between them either with or without loops/cycles, a tree is really a connected graph without cycles
- graphs can have single or double connections between nodes and can be cyclic or acyclic
two ways of to represent a graph:
1. Adjacency List
- most common way to represent a graph
- every node (vertex) stores a list of adjacent vertices
- in the implementation, use a node class and a graph class to store all the nodes (because not all nodes may be accessible in the node class’s children)
- can store the pairings/connections with an array, hash, etc.
2. Adjacency Matrices
- a NxN boolean matrix (where N is the number of nodes) where a true value at matrix[a][b] indicates a connection (edge) from node I to node j. can also use an integer matrix with 0s and 1s
- BFS and other operations are less efficient in a matrix because it’s necessary to iterate through all of the nodes to identify its neighbors

### Tree / Graph Traversal

in-order traversal:
- visits left branch, current node, and then the right branch
```javascript
function inOrderTraversal(node) {
  if (node !== null) {
    inOrderTraversal(node.left);
    visit(node);
    inOrderTraversal(node.right);
  }
}
```

pre-order traversal:
- visits current node before its child nodes, root is always first one visited
```javascript
function preOrderTraversal(node) {
  if (node !== null) {
    visit(node);
    preOrderTraversal(node.left);
    preOrderTraversal(node.right);
  }
}
```

post-order traversal:
- visits current node after its child nodes, root is always last one visited
```javascript
function postOrderTraversal(node) {
  if (node !== null) {
    postOrderTraversal(node.left);
    postOrderTraversal(node.right);
    visit(node);
  }
}
```

BFS (Breadth First Search):
- uses a queue (FIFO) to keep track of nodes visited and nodes to visit
- checks root node and then across its children nodes, then down a level to the next generation, etc.
- add/remove from queue (enqueue, dequeue) with shifting nodes from the front of the array and pushing new ones into the back, does not use recursion, works better iteratively
- generally better for finding shortest path (or any path) between two nodes
- has O(n) time and space complexity
- see notes for algorithm

DFS (Depth First Search):
- uses a stack (LIFO) to track nodes
- adding/removing nodes from end of stack uses pop and push, uses recursion
- often preferred if we want to visit every node in the graph
- see notes for algorithm

## Linked Lists

### Singly-Linked List
A singly-linked list is a data structure that holds a sequence of linked nodes. Each node, in turn, contains data and a pointer, which can point to another node. Since a singly-linked list contains nodes, which can be a separate constructor from a singly-linked list, we outline the operations of both constructors: Node and SinglyList. 
- class Node
  - data stores a value.
  - next points to the next node in the list.
- SinglyList
  - length retrieves the number of nodes in a list.
  - head assigns a node as the head of a list.
  - add(value) adds a node to a list.
  - searchNodeAt(position) searches for a node at n-position in our list.
  - remove(position) removes a node from a list.

Implementation of a Singly-Linked List  
- first define a constructor named Node and then a constructor named SinglyList
- each instance of Node needs a 'data' attribute to store data and a 'next' attribute to point to another node
```javascript
class Node() {
  function appendToTail() {
    let n = this;
    let end = new Node()
    while (n.next !== null) {
      n = n.next
    }
    n.next = end;
  }

  function deleteNode(data) {
    let n = headNode;
    if (n.data === data) {
      head = head.next;
    }
    while (n.next !== null) {
      if (n.next.data == data) {
        n.next = n.next.next
      }
      n = n.next;
    }
  }
}
```

### Doubly-Linked Lists
- similar to single linked lists but are bi-directional, meaning each node also has a “previous” property that refers to the previous node in the line.
- the linked list constructor also has a “tail” property that assigns a node as the tail of the list, at initiation it is null, like the head property.

### The "Runner" Technique
The runner technique is a common linked list method that uses a second pointer to iterate through the linked list with two pointers simultaneously, with one ahead of the other. The "fast" node might be ahead by a fixed amount or it might be hopping multiple nodes for each node that the "slow" node iterates through. For example, to find the length or midpoint of a linked list, you could have one pointer p1 (the fast one) move two elements for every one that p2 (slow pointer) makes. Thus, when p1 hits the end of the linked list, p2 will be at the midpoint.

## Hash Table

A hash table is a data structure that is used to store key/value pairs. It uses a hash function to compute an index into an array in which an element will be inserted or searched. By using a good hash function, hashing can work well. Under reasonable assumptions, the average time required to search for an element in a hash table is O(1).

- Option 1: In case of collision (two or more items sorted into the same index in the table), can use separate chaining / linked lists. Separate chaining is one of the most commonly used collision resolution techniques. It is usually implemented using linked lists. In separate chaining, each element of the hash table is a linked list. To store an element in the hash table you must insert it into a specific linked list. If there is any collision (i.e. two different elements have same hash value) then store both the elements in the same linked list. The cost of a lookup is that of scanning the entries of the selected linked list for the required key. If the distribution of the keys is sufficiently uniform, then the average cost of a lookup depends only on the average number of keys per linked list. For this reason, chained hash tables remain effective even when the number of table entries (N) is much higher than the number of slots.
For separate chaining, the worst-case scenario is when all the entries are inserted into the same linked list. The lookup procedure may have to scan all its entries, so the worst-case cost is proportional to the number (N) of entries in the table.

- Option 2: Open addressing / Linear Probing - In open addressing, instead of in linked lists, all entry records are stored in the array itself. When a new entry has to be inserted, the hash index of the hashed value is computed and then the array is examined (starting with the hashed index). If the slot at the hashed index is unoccupied, then the entry record is inserted in slot at the hashed index else it proceeds in some probe sequence until it finds an unoccupied slot.
The probe sequence is the sequence that is followed while traversing through entries. In different probe sequences, you can have different intervals between successive entry slots or probes. When searching for an entry, the array is scanned in the same sequence until either the target element is found or an unused slot is found. This indicates that there is no such key in the table. The name "open addressing" refers to the fact that the location or address of the item is not determined by its hash value. Linear probing is when the interval between successive probes is fixed (usually to 1). Let’s assume that the hashed index for a particular entry is index. The probing sequence for linear probing will be:
index = index % hashTableSize
index = (index + 1) % hashTableSize
index = (index + 2) % hashTableSize
index = (index + 3) % hashTableSize
- Option 3: Double Hashing

## Heaps

A heap, simply put, is an ordered binary tree. It can be implemented as either a (binary) tree or an array.
- in the binary tree implementation, a max heap has the largest value as the root, meaning every parent node is greater than its children nodes
- for a min heap, the smallest value is at the root, meaning every parent node has a smaller value than its children nodes
- to add an item to the heap, it needs to be added at the left-most value. then in a loop, the new node’s value will be checked against its parent’s value. if the node is bigger than its parent (in a max heap), the two nodes are switched. this comparison-switch occurs up the tree until the condition is satisfied
- when removing an item (usually the root node), root is removed, a comparison between the children is made to determine which should be the root and that node is made the root. this process then continues down the tree for the children nodes until all the spaces are filled
to implement a heap as an array, the nodes are assigned top to bottom, left to right to array indices (so the root is index 0). the formula for finding the children of any given node is parentIndex * 2 + 1 and parentIndex * 2 + 2. Conversely, can find a parent’s index from the child by computing Math.floor(childIndex - 1 / 2)
- function build-max-heap builds a max heap from an unsorted array
- function heapify also builds a max heap but assumes part of array is sorted

### Heap Sort Basics
- unsorted array has build-max-heap called on it. max (root) is swapped with last element in the array and in the tree and is then removed from the tree
- heapify is then called on the partly sorted tree to make it a heap again.
- repeat steps where max root is swapped to end of array and then removed from tree
- this continues until array is fully sorted


Sources:
Cracking The Coding Interview, 6th Edition, by Gayle McDowell, https://github.com/duereg/js-algorithms/blob/master/lib/algorithms/4-searching/depthFirstSearch.js
