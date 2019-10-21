# Technical Interview Algorithm Reference Javascript

Quick reference for data structures and algorithms for technical assessments Leetcode/HackerRank/HackerEarth/etc. Javascript cause there are a million Java ones which my brain struggles to grok.

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
function canFinish(numCourses, prerequisites) {
  let dependencyGraph = {};
  let dependencyCount = Array(numCourses).fill(0);
  let queue = [];

  //Load the dependencyGraph with dependency:course
  //e.g. '0':['1'],'1':['2']
  //Load dependencyCount with dependencyCount per course [i]
  //e.g. [0,1,1,1,1]
  prerequisites.forEach(prereq => {
    const [course, dep] = prereq;
    if (dependencyGraph[dep]) {
      dependencyGraph[dep] = dependencyGraph[dep].concat(course);
    } else {
      dependencyGraph[dep] = [course];
    }
    dependencyCount[course]++;
  });
  //Add courses with 0 dependencies to queue
  for (let i = 0; i < dependencyCount.length; i++) {
    if (dependencyCount[i] == 0) queue.push(i);
  }
  //For each item in queue,
  while (queue.length > 0) {
    let course = queue.shift();
    //load dependencies for a course eg '0':['1']
    let dependencies = dependencyGraph[course];
    if (dependencies) {
      //Decrement dep count for ['1'], if left with 0 add to queue
      dependencies.forEach(dep => {
        dependencyCount[dep]--;
        if (dependencyCount[dep] == 0) queue.push(dep);
      });
    }
  }
  //Check for 0s in dependencyCount, if none true
  for (let i = 0; i < dependencyCount.length; i++) {
    if (dependencyCount[i] !== 0) return false;
  }
  return true;
}
```
