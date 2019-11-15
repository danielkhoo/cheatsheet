# Technical Interview Algorithm Reference Javascript

Quick reference for data structures and algorithms for technical assessments Leetcode/HackerRank/HackerEarth/etc. Javascript cause there are a million Java ones which my brain struggles to grok.

## Big O Time Space / Complexity

### Sorting

| Algo           | Time Complexity (Worst) | Space Complexity |
| -------------- | :---------------------: | ---------------: |
| QuickSort      |           n^2           |           log(n) |
| MergeSort      |         n log n         |                n |
| Bubble Sort    |           n^2           |                1 |
| Insertion Sort |           n^2           |                1 |
| Selection Sort |           n^2           |                1 |

### Data Structures (Worst)

| Algo        | Access |    Search | Insertion |  Deletion | Space Complexity |
| ----------- | :----: | --------: | --------: | --------: | ---------------: |
| Array       |   1    |         n |         n |         n |                n |
| HashTable   |  n/a   | n (avg 1) | n (avg 1) | n (avg 1) |                n |
| Stack       |   n    |         n |         1 |         1 |                n |
| Queue       |   n    |         n |         1 |         1 |                n |
| Linked-List |   n    |         n |         1 |         1 |                n |

## Sorts

### Bubble Sort

```javascript
function bubblesort(input) {
  let sorted = false;
  while (sorted === false) {
    sorted = true;
    let i = 0;
    let j = 1;
    while (j < input.length) {
      if (input[i] > input[j]) {
        [input[i], input[j]] = [input[j], input[i]];
        sorted = false;
      }
      i++;
      j++;
    }
  }
  return input;
}
```

### Selection Sort

```javascript
function selectionSort(input) {
  for (let i = 0; i < input.length; i++) {
    const subArr = input.slice(i); //Slice subArr of unsorted

    //Loop to find index of min item
    let minIndex = 0;
    for (let j = 1; j < subArr.length; j++) {
      if (subArr[j] < subArr[minIndex]) minIndex = j;
    }

    //If min not at start, swap min with item at start of subArr
    if (minIndex > 0) {
      [input[i], input[i + minIndex]] = [input[i + minIndex], input[i]];
    }
  }
  return input;
}
```

### Insertion Sort

```javascript
function insertionSort(input) {
  for (let i = 0; i < input.length; i++) {
    const element = input[i];
    //Loop from end of sorted to 0
    let j;
    for (j = i - 1; j >= 0; j--) {
      if (input[j] < element) break;
      input[j + 1] = input[j]; //Move items to the right
    }
    input[j + 1] = element; //Insert item into space
  }
  return input;
}
```

## Built-in functions

### Destructuring

```javascript
//Filter out elements
const { itemToRemove, ...newObject } = originalObject;

//Swap values
[a, b] = [b, a];
```

### .sort()

```javascript
//ASC
array.sort((a, b) => a - b);
//DESC
array.sort((a, b) => b - a);

//ASC by property
array.sort((a, b) => {
  if (a.id < b.id) return -1;
  else return 1;
});
```

### .map()

```javascript
//Return new arr with elements doubled
const arr = array.map(x => x * 2);
```

### .reduce()

```javascript
//Sum arr elements
const sum = array.reduce((sum, val) => sum + val);
```

### .filter()

```javascript
// Return arr of elements over 0
const arr = array.filter(val => val > 0);
```

### .find()

```javascript
// Return first element over 0 or undefined
array.find(val => val > 0);
```

## Common Questions

### Sieve of Eratosthenes (Generate Primes)

```javascript
function sieve(n) {
  //Generate n sized array with all true
  const primeList = Array(n).fill(true);
  primeList[0] = false; //Set 0 and 1 to false
  primeList[1] = false;

  let p = 2;
  //Starting from 2 set all multiples of a prime to false
  while (p * p < n + 1) {
    for (let i = p * 2; i < n + 1; i += p) {
      if (primeList[i] === true) {
        primeList[i] = false;
      }
    }
    p++;
  }
  return primeList;
}
```

### 2-Array Overlap

```javascript
function overlap(l1, l2) {
  let res = [];
  let i = 0;
  let j = 0;
  //Assuming sorted lists, check for match and increment the pointers
  while (i < l1.length && j < l2.length) {
    if (l1[i] == l2[j]) {
      res.push(l1[i]);
      i++;
      j++;
    } else if (l1[i] < l2[j]) {
      i++;
    } else {
      j++;
    }
  }
  return res;
}
```

### 2 Array Merge Unique Values

```javascript
function mergeArrUniqueValues(l1, l2) {
  let res = l1.concat(l2).sort();

  let i = 0;
  let j = 1;
  while (j < res.length) {
    if (res[i] == res[j]) {
      res[i] = undefined;
    }
    i++;
    j++;
  }

  return res.filter(val => val !== undefined);
}
```

### Google - Watering Plants

```javascript
function waterPlants(plants, capacity1, capacity2) {
  let i = 0;
  let j = plants.length - 1;
  let refillCount = 0;
  let watercan1 = 0;
  let watercan2 = 0;
  while (i <= j) {
    if (i == j) {
      if (watercan1 + watercan2 < plants[i]) {
        refillCount++;
      }
      break;
    }
    if (watercan1 < plants[i]) {
      watercan1 = capacity1;
      refillCount++;
    }
    watercan1 -= plants[i];

    if (watercan2 < plants[j]) {
      watercan2 = capacity2;
      refillCount++;
    }
    watercan2 -= plants[j];

    i++;
    j--;
  }
  return refillCount;
}
```

### Google - Rotate Dominoes

```javascript
function minDominoRotations(A, B) {
  let valid = [];
  let candidates = {};
  candidates[A[0]] = 1;
  candidates[B[0]] = 1;
  for (let i = 1; i < A.length; i++) {
    if (candidates[A[i]] == i) {
      candidates[A[i]] += 1;
    }
    if (candidates[B[i]] == i) {
      candidates[B[i]] += 1;
    }
  }

  Object.keys(candidates).forEach(key => {
    if (candidates[key] == A.length) valid.push(key);
  });

  if (valid.length == 0) {
    return -1;
  }
  let swapCount1 = 0;
  let val1 = valid[0];
  for (let j = 0; j < A.length; j++) {
    if (A[j] != val1) swapCount1++;
  }

  let swapCount2 = 0;
  for (let j = 0; j < A.length; j++) {
    if (B[j] != val1) swapCount2++;
  }

  return swapCount1 > swapCount2 ? swapCount2 : swapCount1;
}
```

### 3sum

```javascript
function threeSum(input) {
  let searchHashmap = {};
  let resHashmap = {};
  for (let i = 0; i < input.length; i++) {
    for (let j = 0; j < input.length; j++) {
      if (i == j) {
        continue;
      }
      //searchhashmap key is value needed for sum to be 0
      let temp = 0 - input[i] - input[j];
      //Check if index j matches previously saved hashes
      if (searchHashmap[input[j]]) {
        const [indexI, indexJ] = searchHashmap[input[j]];
        if (j != indexI && j != indexJ) {
          //Sort arr of 3 nums and use as key to prevent duplicates
          const arr = [input[j], input[indexI], input[indexJ]].sort((a, b) => a - b);
          resHashmap[arr] = arr;
        }
      }
      searchHashmap[temp] = [i, j];
    }
  }
  return Object.values(resHashmap);
}
```

### Reverse Linked List

```javascript
function reverseLinkedList(head) {
  let curr = head;
  let prev = null;
  while (curr != null) {
    const nextTemp = curr.next;
    curr.next = prev;
    prev = curr;
    curr = nextTemp;
  }
  return prev;
}
```

### Reverse Linked List Between Indices

```javascript
function reverseLinkedListBetween(head, m, n) {
  let curr = head;
  let prev = null;

  while (m >= 1) {
    prev = curr;
    curr = curr.next;
    m--;
    n--;
  }
  let con = prev;
  let tail = curr;
  while (n >= 0) {
    let nextTemp = curr.next;
    curr.next = prev;
    prev = curr;
    curr = nextTemp;
    n--;
  }
  if (con) {
    con.next = prev;
  } else {
    head = prev;
  }
  tail.next = curr;
  return head;
}
```

### Count Continous Sections in Linked List

```javascript
function linkedListContinous(head, G) {
  let curr = head;
  let hashmap = {};
  let count = 0;
  for (let i = 0; i < G.length; i++) {
    hashmap[G[i]] = 1;
  }
  let continous = false;
  while (curr !== null) {
    let val = curr.val;
    if (hashmap[val]) {
      delete hashmap[val];
      if (continous === false) {
        count++;
        continous = true;
      }
    } else {
      continous = false;
    }

    curr = curr.next;
  }
  return count;
}
```

### Max Contiguous Sum in Array

```javascript
function maxContigous(input) {
  let maxEndingHere = input[0];
  let res = input[0];
  for (let i = 1; i < input.length; i++) {
    maxEndingHere = Math.max(maxEndingHere + input[i], input[i]);
    res = Math.max(res, maxEndingHere);
  }
  return res;
}
```

### Dependency Graph

```javascript
function findValidOrder(numCourses, prerequisites) {
  let prereqGraph = {};
  let courseDependencyCount = Array(numCourses).fill(0);
  let queue = [];
  let order = [];

  //Load the prereqGraph with dependency:course
  //e.g. '0':['1'],'1':['2']
  //Load courseDependencyCount with courseDependencyCount per course [i]
  //e.g. [0,1,1,1,1]
  prerequisites.forEach(prereq => {
    const [course, dep] = prereq;
    if (prereqGraph[dep]) {
      prereqGraph[dep] = prereqGraph[dep].concat(course);
    } else {
      prereqGraph[dep] = [course];
    }
    courseDependencyCount[course]++;
  });
  //Add courses with 0 dependencies to queue
  for (let i = 0; i < courseDependencyCount.length; i++) {
    if (courseDependencyCount[i] == 0) queue.push(i);
  }
  //For each item in queue,
  while (queue.length > 0) {
    let course = queue.shift();
    order.push(course);
    //load dependencies for a course eg '0':['1']
    let dependencies = prereqGraph[course];
    if (dependencies) {
      //Decrement dep count for ['1'], if left with 0 add to queue
      dependencies.forEach(dep => {
        courseDependencyCount[dep]--;
        if (courseDependencyCount[dep] == 0) queue.push(dep);
      });
    }
  }
  //Check for 0s in courseDependencyCount, if none true
  for (let i = 0; i < courseDependencyCount.length; i++) {
    if (courseDependencyCount[i] !== 0) return [];
  }
  return order;
}
```

### Buy and Sell Stock

```javascript
const input = [2, 1, 4, 6, 3];
function maxProfit(prices) {
  if (input.length == 0) return 0;
  let min;
  let max;
  let maxDiff = 0;
  for (let i = 0; i < prices.length; i++) {
    const price = prices[i];
    //If no min set min & max to price
    if (min == undefined) min = max = price;
    //If price lower than min, reset min/max
    if (price < min) min = max = price;
    //If price more than max, set as new max
    if (price > max) max = price;

    //If diff is greater than current, set as new max diff
    const diff = price - min;
    if (diff > maxDiff) maxDiff = diff;
  }
  return maxDiff;
}
```

### Depth First Search

Number of distinct items in a 2D grid

```javascript
function numIslands(grid) {
  let count = 0;
  let height = grid.length;
  let length = grid[0].length;

  for (let y = 0; y < height; y++) {
    for (let x = 0; x < length; x++) {
      if (grid[y][x] == '0') continue;
      dfs(x, y);
      count++;
    }
  }
  return count;
  function dfs(x, y) {
    if (x < 0 || y < 0 || x == length || y == height) return;
    if (grid[y][x] == '0') return;
    grid[y][x] = '0';

    dfs(x - 1, y);
    dfs(x + 1, y);
    dfs(x, y - 1);
    dfs(x, y + 1);
  }
}
```
