# Algorithms

These are basic implementations of some popular sorting, traversal and conceptual algorithms. Some of the examples are implemented in multiple ways and/or programming languages.

## Merge Sort

From InteractivePython.org: "Merge sort is a recursive algorithm that continually splits a list in half. If the list is empty or has one item, it is sorted by definition (the base case). If the list has more than one item, we split the list and recursively invoke a merge sort on both halves. Once the two halves are sorted, the fundamental operation, called a merge, is performed. Merging is the process of taking two smaller sorted lists and combining them together into a single, sorted, new list."

Because merge sort has to iterate through the entire input array O(log n) times, the time complexity for it is O(n log n).

In JavaScript:
```javascript
function mergeSort(array, func) {
  if (array.length <= 1) return array;

  if (!func) func = (left, right) => {
    return left < right ? -1 : left > right ? 1 : 0;
  };

  const midpoint = Math.floor(array.length / 2);
  const sortedLeft = mergeSort(array.slice(0, midpoint), func);
  const sortedRight = mergeSort(array.slice(midpoint), func);

  return merge(sortedLeft, sortedRight, func);
};

function merge(arr1, arr2, func) {
  let merged = [];

  while (arr1.length && arr2.length) {
    switch (func(arr1[0], arr2[0])) {
      case -1:
      merged.push(arr1.shift());
      break;
      case 0:
      merged.push(arr1.shift());
      break;
      case 1:
      merged.push(arr2.shift());
      break;
    }
  }

  merged = merged.concat(arr1);
  merged = merged.concat(arr2);

  return merged;
};
```

## Quick Sort

Like Merge Sort, Quick Sort is a divide and conquer and comparison sorting algorithm. It picks an element as a pivot and partitions the given array around the chosen pivot. There are many different versions of quickSort that pick pivot in different ways:
- Always pick first element as pivot
- Always pick last element as pivot
- Pick a random element as pivot
- Pick median as pivot
The array continues to recursively subdivide itself and run comparisons with a selected element and the pivot until the full array is sorted. Quick Sort typically runs in O(n log n) time (worst case O(n^2)) and doesn't require any additional space unlike Merge Sort.

In JavaScript:
```JavaScript
function quickSort (array, func) {
  if (array.length < 2) return array;

  if (!func) {
    func = (x, y) => {
      if (x < y) return - 1;
      return 1;
    };
  }

  const pivot = array[0];
  let left = array.slice(1).filter( (el) => func(el, pivot) === -1);
  let right = array.slice(1).filter( (el) => func(el, pivot) !== -1);
  left = quickSort(left, func);
  right = quickSort(right, func);

  return left.concat([pivot]).concat(right);
};
```

## Binary Search

Binary Search, also known as half-interval search, logarithmic search, or binary chop, is a search algorithm that finds the position of a target value within a sorted array. Binary search compares the target value to the middle element of the array; if they are unequal, the half in which the target cannot lie is eliminated and the search continues on the remaining half until it is successful. If the search ends with the remaining half being empty, the target is not in the array.

Binary search runs at worst in O(log n) time.

In JavaScript:
```javascript
function binarySearch (array, target) {
  if (array.length === 0) return null;
  const mid = Math.floor(array.length / 2);

  if (array[mid] === target) {
    return mid;
  } else if (array[mid] > target) {
    return binarySearch(array.slice(0, mid), target);
  } else {
    const result = binarySearch(array.slice(mid + 1), target);
    return result === null ? result : mid + 1 + result;
  }
}
```

## Bubble Sort

"Bubble sort, sometimes referred to as sinking sort, is a simple sorting algorithm that repeatedly steps through the list to be sorted, compares each pair of adjacent items and swaps them if they are in the wrong order. The pass through the list is repeated until no swaps are needed, which indicates that the list is sorted. The algorithm, which is a comparison sort, is named for the way smaller or larger elements "bubble" to the top of the list. Bubble sort can be practical if the input is in mostly sorted order with some out-of-order elements nearly in position." - adapted from Wikipedia.

Bubble Sort runs in O(n^2) time but has O(1) space complexity.

```javascript
function bubbleSort(array, func) {
  let sorted = false;

  if (!func) {
    func = (x, y) => {
      if (x <= y) return -1;
      return 1;
    };
  }

  while (!sorted) {
    sorted = true;
    for (let i = 0; i < array.length; i++) {
      if (i + 1 === array.length) continue;

      if (func(array[i], array[i + 1]) === 1) {
        sorted = false;
        let current = array[i], next = array[i + 1];
        array[i] = next, array[i + 1] = current;
      }
    }
  }

  return array;
};
```

## Heap Sort

Heap Sort is a comparison-based sorting algorithm; it divides its input into a sorted and an unsorted region, and it interactively shrinks the unsorted region by extracting the largest element and moving that to the sorted region. The improvement consists of the use of a heap data structure rather than a linear-time search to find the maximum. It runs in worst-case O(n log n) runtime and runs in-place (O(1) space complexity).

```JavaScript
let arrayLength;
/* to create MAX  array */  
function heapify(input, i) {
  let left = 2 * i + 1;
  let right = 2 * i + 2;
  let max = i;

  if (left < arrayLength && input[left] > input[max]) {
      max = left;
  }

  if (right < arrayLength && input[right] > input[max])     {
      max = right;
  }

  if (max != i) {
      swap(input, i, max);
      heapify(input, max);
  }
}

function swap(input, idxA, idxB) {
  let temp = input[idxA];

  input[idxA] = input[idxB];
  input[idxB] = temp;
}

function heapSort(input) {

  arrayLength = input.length;

  for (let i = Math.floor(arrayLength / 2); i >= 0; i -= 1)      {
      heapify(input, i);
    }

  for (i = input.length - 1; i > 0; i--) {
      swap(input, 0, i);
      arrayLength--;


      heapify(input, 0);
  }
}
```

## Fibonacci Sequence and Sum

From Wikipedia: By definition, the first two numbers in the Fibonacci sequence are either 1 and 1, or 0 and 1, depending on the chosen starting point of the sequence, and each subsequent number is the sum of the previous two.

### Iterative Sum
- runs in O(n) time and O(1) space complexity

```javascript
function fibIter(n) {
    let a = 0, b = 1, temp;
    for(let i = 2; i <= n; i++) {
        f = a + b;
        a = b;
        b = f;
    }
    return f;
};
```

### Recursive Sum
- runs in O(2^n) time (exponential) and O(n) space

```javascript
function fibRec(num) {
  if (num <= 1) return 1;

  return fibRec(num - 1) + fibRec(num - 2);
}
```

### Recursive Sum w/ Memoization
- runs in O(2n) => O(n) time with O(n) space

```javascript
function fibMemo(num, memo) {
  memo = memo || {};

  if (memo[num]) return memo[num];
  if (num <= 1) return 1;

  return memo[num] = fibMemo(num - 1, memo) + fibMemo(num - 2, memo);
}
```

## Tree Traversal - Breadth-First Search

(See data_structures for description)

```javascript
//first write node object as a POJO like this:
let node1 = {
  data: 1,
  left: "referenceToLeftNode",
  right: "referenceToRightNode"
};

function breadthFirstSearch(rootNode) {
  // Check that a root node exists.
  if (rootNode === null) {
    return;
  }

  // Create our queue and push our root node into it.
  let queue = [];
  queue.push(rootNode);

  // Continue searching through as queue as long as it's not empty.
  while (queue.length > 0) {
    // Create a reference to currentNode, at the top of the queue.
    let currentNode = queue[0];

    // Print the data of the node. This simulates visiting / checking the node.
    console.log(currentNode.data);

    // If currentNode has a left child node, add it to the queue.
    if (currentNode.left !== null) {
      queue.push(currentNode.left);
    }
    // If currentNode has a right child node, add it to the queue.
    if (currentNode.right !== null) {
      queue.push(currentNode.right);
    }
    // Remove the currentNode from the queue.
    queue.shift();
  }

  // Continue looping through the queue until it's empty!
}
```
## Tree Traversal - Depth-First Search

(See data_structures for description)

```javascript
function depthFirstSearch(node) {
  // Check that a node exists.
  if (node === null) {
    return;
  }

  // Print the data of the node. This simulates visiting / checking the node.
  console.log(node.data);

  // Pass in a reference to the left child node to depthFirstSearch.
  // Then, pass reference to the right child node to depthFirstSearch.
  depthFirstSearch(node.left);
  depthFirstSearch(node.right);
}
```


Sources:
http://interactivepython.org/runestone/static/pythonds/SortSearch/TheMergeSort.html, https://www.geeksforgeeks.org/quick-sort/, https://en.wikipedia.org/wiki/Binary_search_algorithm, https://en.wikipedia.org/wiki/Bubble_sort, https://www.w3resource.com/javascript-exercises/searching-and-sorting-algorithm/searching-and-sorting-algorithm-exercise-3.php,https://medium.com/developers-writing/fibonacci-sequence-algorithm-in-javascript-b253dc7e320e, https://www.thepolyglotdeveloper.com/2015/01/fibonacci-sequence-printed-javascript/
