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


Sources:
Cracking The Coding Interview, 6th Edition, by Gayle McDowell, https://github.com/duereg/js-algorithms/blob/master/lib/algorithms/4-searching/depthFirstSearch.js
