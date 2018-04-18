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
  let right = array.slice(1).filter( (el) => func(el, pivot) != -1);
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


Sources:
http://interactivepython.org/runestone/static/pythonds/SortSearch/TheMergeSort.html, https://www.geeksforgeeks.org/quick-sort/, https://en.wikipedia.org/wiki/Binary_search_algorithm,
