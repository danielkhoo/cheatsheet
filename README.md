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
