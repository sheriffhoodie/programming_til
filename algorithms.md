These are basic implementations of some popular sorting, traversal and conceptual algorithms. Some of the examples are implemented in multiple ways and/or programming languages.

## Merge Sort

From InteractivePython.org: "Merge sort is a recursive algorithm that continually splits a list in half. If the list is empty or has one item, it is sorted by definition (the base case). If the list has more than one item, we split the list and recursively invoke a merge sort on both halves. Once the two halves are sorted, the fundamental operation, called a merge, is performed. Merging is the process of taking two smaller sorted lists and combining them together into a single, sorted, new list.""

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

Sources:
http://interactivepython.org/runestone/static/pythonds/SortSearch/TheMergeSort.html,
