# InterviewPrep

## Fib

### Recursive

```javascript
// Time - O(2^n)
// Space - O(n)
const fib = n => {
  if (n <= 2) {
    return 1;
  } else {
    return fib(n - 1) + fib(n - 2);
  }
};
fib(6);
```

### Iterative

```javascript
// Time - O(n)
// Space - O(1)
const fib2 = n => {
  var state = [0, 1];
  for (let x = 0; x < n; x++) {
    state = [state[1], state[0] + state[1]];
  }
  return state[0];
};

fib2(6);
```

## Factorial

### Recursive

```javascript
// Time - O(n)
// Space - O(n)
const factorial = n => {
  if (n <= 2) return n;
  return n * factorial(n - 1);
};

factorial(10);
```

## Bubble Sort

```javascript
// Time - O(n^2)
// Space - O(1)
// Stable
const bubbleSort = (
  numbers,
  areInIncreasingOrder = (a, b) => {
    return a > b;
  }
) => {
  var noSwaps = true;
  for (let endIndex = numbers.length; endIndex > 0; endIndex--) {
    for (let currentIndex = 0; currentIndex < endIndex; currentIndex++) {
      const nextIndex = currentIndex + 1;
      if (areInIncreasingOrder(numbers[currentIndex], numbers[nextIndex])) {
        const temp = numbers[nextIndex];
        numbers[nextIndex] = numbers[currentIndex];
        numbers[currentIndex] = temp;
        noSwaps = false;
      }
    }
    if (noSwaps) return numbers;
  }
  return numbers;
};

bubbleSort([4, 6, 5, 7], (a, b) => {
  return a < b;
});
```

## Insertion Sort

```javascript
// Time - O(n^2)
// Space - O(1)
// Stable
const insertionSort = numbers => {
  for (let currentIndex = 0; currentIndex < numbers.length; currentIndex++) {
    for (let swapIndex = currentIndex + 1; swapIndex > 0; swapIndex--) {
      let previousIndex = swapIndex - 1;
      if (numbers[previousIndex] > numbers[swapIndex]) {
        var temp = numbers[previousIndex];
        numbers[previousIndex] = numbers[swapIndex];
        numbers[swapIndex] = temp;
      } else {
        break;
      }
    }
  }

  return numbers;
};

insertionSort([6, 4, 5, 4, 3, 8, 7, 9, 8, 7, 2, 3, 2, 1]);
```

## Merge Sort

```javascript
// Time - O(n log n)
// Space - O(n)
// Stable
const mergeSort = nums => {
  if (nums.length <= 1) return nums;
  let middle = nums.length / 2;
  let left = mergeSort(nums.slice(0, middle));
  let right = mergeSort(nums.slice(middle, nums.length));
  return merge(left, right);
};

const merge = (left, right) => {
  let result = [];
  let leftIndex = 0;
  let rightIndex = 0;

  while (leftIndex < left.length && rightIndex < right.length) {
    let leftElement = left[leftIndex];
    let rightElement = right[rightIndex];

    if (leftElement > rightElement) {
      result.push(rightElement);
      rightIndex += 1;
    } else if (leftElement < rightElement) {
      result.push(leftElement);
      leftIndex += 1;
    } else {
      result.push(leftElement);
      result.push(rightElement);
      leftIndex += 1;
      rightIndex += 1;
    }
  }

  if (leftIndex < left.length) {
    return result.concat(left.slice(leftIndex, left.length));
  }

  return result.concat(right.slice(rightIndex, right.length));
};

mergeSort([7, 5, 6, 5, 4, 3, 4, 3, 2, 1]);
```

## Median element of two sorted arrays

```javascript
// Time - O(n)
// Space - O(n)
let a = [1, 2, 3];
let b = [1, 4, 5, 6, 7, 8];

const median = (arr1, arr2) => {
  let medianIndex = (arr1.length + arr2.length) / 2;
  let leftIndex = 0;
  let rightIndex = 0;
  let result = [];

  while (leftIndex + rightIndex < medianIndex && rightIndex < arr2.length) {
    let leftElement = arr1[leftIndex];
    let rightElement = arr2[rightIndex];

    if (leftElement < rightElement) {
      leftIndex += 1;
      result.push(leftElement);
    } else if (leftElement > rightElement) {
      rightIndex += 1;
      result.push(rightElement);
    } else {
      leftIndex += 1;
      rightIndex += 1;
      result.push(leftElement);
      result.push(rightElement);
    }

    if (medianIndex == result.length) {
      return result[medianIndex];
    }
  }

  if (leftIndex < arr1.length) {
    result = result.concat(arr1.slice(leftIndex, arr1.length));
  } else {
    result = result.concat(arr2.slice(rightIndex, arr2.length));
  }

  let middle = Math.floor(result.length / 2);
  return result[middle];
};

median(a, b);
```

## Quick Sort

```javascript
// Time - O(n log n)
// Space - O(log n)
// Unstable

const quickSort = numbers => {
  if (numbers.length <= 1) return numbers;

  const randomIndex = Math.floor(Math.random() * numbers.length);
  const pivot = numbers[randomIndex];

  const left = numbers.filter(number => {
    return number < pivot;
  });

  const pivots = numbers.filter(number => {
    return number == pivot;
  });

  const right = numbers.filter(number => {
    return number > pivot;
  });

  return [...quickSort(left), ...pivots, ...quickSort(right)];
};

quickSort([5, 3, 6, 7, 8, 7, 6, 3, 2, 1, 2, 4, 3, 5, 6, 5, 4, 3, 4, 3, 2]);
```

## ArrayList

```javascript
class ArrayList {
  constructor() {
    this.length = 0;
    this.data = {};
  }

  length() {
    return this.length;
  }

  push(element) {
    this.data[this.length] = element;
    this.length++;
  }

  pop() {
    this.delete(this.length - 1);
  }

  get(index) {
    return this.data[index];
  }

  delete(index) {
    const element = this.data[index];
    this._collapse(index);
    return element;
  }

  _collapse(index) {
    for (let i = index; i < this.length; i++) {
      this.data[i] = this.data[i + 1];
    }
    delete this.data[this.length];
    this.length--;
  }
}

const array = new ArrayList();
array.push(99);
array.push(76);
array.push(54);
array.push(32);
array.push(34);
array.pop();
array.get(2);
array;
array.length;
array;
array.delete(5);
array;
array.length;
array;
```

## BST

```javascript
class Node {
  constructor(value, left = null, right = null) {
    this.value = value;
    this.left = left;
    this.right = right;
  }
}

class Tree {
  constructor() {
    this.root = null;
  }

  add(value) {
    if (!this.root) {
      this.root = new Node(value);
      return;
    }

    let currentNode = this.root;
    while (true) {
      if (currentNode.value > value) {
        if (currentNode.left) {
          currentNode = currentNode.left;
        } else {
          currentNode.left = new Node(value);
          return;
        }
      } else {
        if (currentNode.right) {
          currentNode = currentNode.right;
        } else {
          currentNode.right = new Node(value);
          return;
        }
      }
    }
  }
}

const bst = new Tree();
bst;
bst.add(1);
bst.add(2);
bst;
```

## Memoized Factorial

```javascript
const memoize = (cb) => {
  const memo = {}
  return (n) => {
    console.log(memo)
    if (n in memo) {
      return memo[n]
    } else {
      const result = cb(n)
      memo[n] = result
      return result
    }
  }
}

const factorial = memoize((n) => {
  if (n === 0) return 1
  return n * factorial(n - 1)
})
```

## Make Change
```javascript
const makeChange = amount => {
  if (amount < 5) return null

  let runningAmount = 0
  let coins = [25, 10, 5]
  let coinsUsed = []

  while (runningAmount < amount) {
    for (let coin of coins) {
      if (runningAmount + coin <= amount) {
        runningAmount += coin
        coinsUsed.push(coin)
        break
      }
    }
  }
  return coinsUsed
}

makeChange(35)
```

### 3 Stacks
```javascript
// 3 Stacks

class NStacks {
  constructor(numberOfStacks, capacity) {
    this.numberOfStacks = numberOfStacks
    this.capacity = capacity
    this.size = numberOfStacks * capacity
    this.storage = []
    this.currIndices = []
    
    for (let i = 0; i < this.numberOfStacks; i++) {
      const startIndex = i * this.capacity
      this.currIndices.push(startIndex)
    }
  }
  
  push(stackNumber, value) {
    if (stackNumber >= this.numberOfStacks) {
      return
    }
    
    if (this.isFull(stackNumber)) {
      return
    }
    
    const currIndex = this.currIndices[stackNumber]
    this.storage[currIndex] = value
    this.currIndices[stackNumber]++
  }
  
  pop(stackNumber) {
    if (stackNumber >= this.numberOfStacks) {
      return
    }
    
    if (this.isEmpty(stackNumber)) {
      return null
    }
    
    this.currIndices[stackNumber]--
    const currIndex = this.currIndices[stackNumber]
    const value = this.storage[currIndex]
    delete this.storage[currIndex]
    return value
  }
  
  isFull(stackNumber) {
    const currIndex = this.currIndices[stackNumber]
    const maxCapacity = ((stackNumber + 1) * this.capacity) - 1
    return currIndex > maxCapacity
  }
  
  isEmpty(stackNumber) {
    const currIndex = this.currIndices[stackNumber] - 1
    const minCapacity = stackNumber * this.capacity
    return currIndex < minCapacity
  }
}


const n = new NStacks(1, 2)
n.push(1, 1)
n.push(1, 2)
n.push(1, 3)
n.push(0, 4)
n.push(0, 5)
n.push(0, 9)
console.log(n)
n.pop(0)
console.log(n)
n.pop(0)
console.log(n)
// n.pop(0)
// n.push(3, 4)
// n.push(2, 5)
// n.push(2, 9)
// console.log(n)
n.pop(2)
console.log(n)
```