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

// Iterative
const arr = [3, 6, 5, 7, 9, 8, 4, 3, 1]

const mergeSort = arr => {
  if (arr.length <= 1) return arr
  
  arr = arr.map(ele => [ele])
  
  while (arr.length > 1) {
    let result = []
    for (let i = 0; i < arr.length; i += 2) {
      let next = i + 1
      result.push(merge(arr[i], arr[next]))
    }
    arr = result
  }
  return arr[0]  
}

const merge = (left, right) => {  
  if (!left) return right
  if (!right) return left
  
  let result = []
  let leftIndex = 0
  let rightIndex = 0
  
  while (leftIndex < left.length && rightIndex < right.length) {
    let leftElement = left[leftIndex]
    let rightElement = right[rightIndex]
    
    if (leftElement < rightElement) {
      result.push(leftElement)
      leftIndex++
    } else if (leftElement > rightElement) {
      result.push(rightElement)
      rightIndex++
    } else {
      result.push(leftElement)
      result.push(rightElement)
      leftIndex++
      rightIndex++
    }
  }
  
  if (leftIndex < left.length) {
    result = [...result, ...left.slice(leftIndex)]
  } else {
    result = [...result, ...right.slice(rightIndex)]
  }
  return result
}

mergeSort(arr)
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

const random = (min, max) => {
  return Math.floor(Math.random() * (max - min + 1)) + min
}

const quickSort = (arr, start, end) => {
  if (start >= end) return

  let low = start || 0
  let high = end || arr.length - 1
  let randomIndex = random(low, high)
  swap(arr, randomIndex, high)

  let pivot = partition(arr, low, high)
  quickSort(arr, low, pivot - 1)
  quickSort(arr, pivot + 1, high)
}

const partition = (arr, start, end) => { 
  let pivot = end
  let i = start - 1
  
  for (let j = start; j < end; j++) {
    if (arr[j] <= arr[pivot]) {
      i++
      swap(arr, j, i)
    }
  }

  swap(arr, i + 1, pivot)
  return i + 1
}

const swap = (arr, first, second) => {
  let temp = arr[first]
  arr[first] = arr[second]
  arr[second] = temp
}

const arr = [6, 2, 5, 3, 8, 7, 1, 4]
quickSort(arr)
console.log(arr)
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

```javascript
class NStacks {
  constructor(stackCount, stackSize) {
    this.stackCount = stackCount
    this.stackSize = stackSize
    this.storage = Array(stackCount * stackSize).fill(null)
    this.currentIndices = []
    
    for (let i = 0; i < stackCount; i++) {
      this.currentIndices.push(i * stackSize)
    }
  }
  
  push(value, stackNumber) {
    if (!this.includesStack(stackNumber)) return
    if (this.isFull(stackNumber)) return
    let currentIndex = this.currentIndices[stackNumber]
    this.currentIndices[stackNumber]++
    this.storage[currentIndex] = value
  }
  
  pop(stackNumber) {
    if (!this.includesStack(stackNumber)) return
    if (this.isEmpty(stackNumber)) return
    this.currentIndices[stackNumber]--    
    let currentIndex = this.currentIndices[stackNumber]
    this.storage[currentIndex] = null
  }
  
  isFull(stackNumber) {
    let max = (stackNumber * this.stackSize) + this.stackSize
    let currentIndex = this.currentIndices[stackNumber]
    return currentIndex >= max
  }
  
  isEmpty(stackNumber) {
    let min = stackNumber * this.stackSize
    let currentIndex = this.currentIndices[stackNumber]
    return currentIndex <= min
  }
  
  includesStack(stackNumber) {
    return stackNumber < this.stackCount
  }
}

const stacks = new NStacks(3, 3)
stacks.push(1, 1)
stacks.push(3, 1)
stacks.push(6, 1)
stacks.push(2, 2)
stacks.push(3, 3)
stacks.push(4, 1)
stacks.pop(0)
stacks
stacks.pop(1)
stacks
stacks.pop(1)
stacks
stacks.pop(3)
stacks
```

## Stack getMin
```javascript
class Stack {
  constructor() {
    this._storage = []
    this._min = []
  }
  
  push(value) {
    const currMin = this._min[this._min.length - 1]
    
    if (!currMin) {
      this._min.push(value)
    }
    
    if (currMin > value) {
      this._min.push(value)
    }
    
    this._storage.push(value)
  }
  
  pop() {
    const value = this._storage.pop()
    const currMin = this._min[this._min.length - 1]
    
    if (value == currMin) {
      this._min.pop()
    }
    
    return value
  }
  
  getMin() {
    return this._min[this._min.length - 1]
  }
}

const stack = new Stack()
stack.push(0)
stack.push(2)
stack.push(3)
stack.push(1)
stack.push(5)
stack.push(2)
stack.push(3)
stack.push(1)
console.log(stack)
stack.pop()
console.log(stack)
stack.pop()
console.log(stack)
stack.pop()
console.log(stack)
```

## Queue from two stacks
```javascript
class Queue {
  constructor() {
    this._leftStack = [];
    this._rightStack = [];
  } 
  
  enqueue(value) {
    this._leftStack.push(value);
  }
  
  dequeue() {
    if (this._rightStack.length == 0) {
      for (let element of this._leftStack) {
        this._rightStack.push(element)
      }
      this._leftStack = []
    }
    return this._rightStack.pop()
  }
}

const queue = new Queue()
queue.enqueue(1)
queue.enqueue(2)
queue.enqueue(3)
queue.enqueue(4)
console.log(queue)
queue.dequeue()
queue.dequeue()
queue.dequeue()
queue.dequeue()
queue.dequeue()
```

## Sort Stack
```javascript
class Stack {
  constructor() {
    this._storage = [];
  }
  
  push(value) {
    this._storage.push(value);
  } 
  
  pop() {
    return this._storage.pop()
  }
  
  sort() {
    const buffer = []
    let temp = this._storage.pop()
    
    while (temp) {      
      let topValue = buffer[buffer.length - 1]
        while (topValue < temp) {
          let value = buffer.pop()
          this._storage.push(value)
          topValue = buffer[buffer.length - 1]
        }  
        buffer.push(temp)
        temp = this._storage.pop()
    }
    
    this._storage = buffer
  }
}

const stack = new Stack()
stack.push(9)
stack.push(8)
stack.push(2)
stack.push(4)
stack.push(1)
stack.push(99)
console.log(stack)
stack.sort()
console.log(stack)
stack.pop()
```

## Delete Middle Node
```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
  }
}

class LinkedList {
  constructor() {
    this.head = null
  }
  
  push(value) {
    const node = new Node(value)
    node.next = this.head
    this.head = node
  }
  
  delete(node) {
    let midNode = node
    let nextNode = node.next
    midNode.value = nextNode.value
    midNode.next = midNode.next.next
  }
  
  print() {
    let head = this.head
    while (head) {
      console.log(head.value)
      head = head.next
    }
  }
  
  get(i) {
    let head = this.head
    let currentIndex = 0
    
    while (head && currentIndex < i) {
      head = head.next
      currentIndex++
    }
    return head
  }
}

const ll = new LinkedList()
ll.push(1)
ll.push(2)
ll.push(3)
ll.push(4)
ll.push(5)
const t = ll.get(3)
ll.print()
console.log("----")
ll.delete(t)
ll.print()
```


## CCI 2.1
```javascript
const removeDups = list => {
  if (!list || !list.next) return list
  
  let curr = list
  
  while (curr) {
    let runner = curr
    while (runner.next) {
      if (runner.next.val === curr.val) {
        runner.next = runner.next.next
      } else {
        runner = runner.next
      }
    }
    curr = curr.next
  }
  
  return list
}
```

```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
  }
}

class LinkedList {
  constructor() {
    this.head = null
  }
  
  push(value) {
    const node = new Node(value)
    node.next = this.head
    this.head = node
  }
  
  removeDups() {
    const seenValues = new Set();
    let curr = this.head;
    let runner = curr.next;
    seenValues.add(curr.value)
    
    while (runner) {
      if (!seenValues.has(runner.value)) {
        seenValues.add(runner.value)
        runner = runner.next
        curr = curr.next
      } else {
        runner = runner.next
        curr.next = runner
      }
    }
  }
  
  print() {
    let head = this.head
    while (head) {
      console.log(head.value)
      head = head.next
    }
  }
}

const ll = new LinkedList()
ll.push(2)
ll.push(2)
ll.push(3)
ll.push(4)
ll.push(5)
ll.push(2)
ll.push(3)
ll.push(2)
ll.print()
console.log("----")
ll.removeDups()
console.log("----")
ll.print()
```

## CCI 2.2
```javascript
class Node {
  constructor(val) {
    this.val = val
    this.next = null
  }
}

const node1 = new Node(1)
const node2 = new Node(2)
const node3 = new Node(3)
const node4 = new Node(4)
const node5 = new Node(5)
const node6 = new Node(6)
const node7 = new Node(7)
const node8 = new Node(8)
node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7
node7.next = node8

const kToLast = (list, k) => {
  if (!list) return list
  if (k < 0) return null
  
  let fast = list
  let slow = list
  while (k--) {
    if (!fast) return null  
    fast = fast.next
  }
  
  while (fast.next) {
    fast = fast.next
    slow = slow.next
  }
  return slow
}

kToLast(node1, 19)

const kToLast = (list, k) => {
  if (k < 0) return null
  if (!list) return [null, 0]
  
  let [node, count] = kToLast(list.next, k)
  
  if (count === k + 1) {
    return [node, count]
  }
  
  return [list, count + 1]
}
```

## Reverse Linked List
```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
  }
}

class LinkedList {
  constructor() {
    this.head = null
  }
  
  push(value) {
    const node = new Node(value)
    node.next = this.head
    this.head = node
  }
  
  reverseIterative() {
    let current = this.head;
    let previous;
    let next;
    
    while (current) {
      next = current.next
      current.next = previous
      previous = current
      current = next
    }
    this.head = previous
  }
  
  reverseRecursive() {
    this.head = this._reverseRecursive(this.head)
  }
  
  _reverseRecursive(node) {
    if (!node) return null
    if (!node.next) return node
    let head = this._reverseRecursive(node.next)
    node.next.next = node
    node.next = null
    return head
  }
  
  print() {
    let head = this.head
    while (head) {
      console.log(head.value)
      head = head.next
    }
  }
}

const ll = new LinkedList()
ll.push(1)
ll.push(2)
ll.push(3)
ll.push(4)
ll.push(5)
ll.reverseIterative()
ll.print()
console.log("---")
ll.reverseRecursive()
ll.print()
```

## Has a Cycle
```javascript
// cycle



class Node {
  constructor(value) {
    this.value = value
    this.next = null
  }
}

class LinkedList {
  constructor() {
    this.head = null
    this.tail = null
  }
  
  push(value) {
    const node = new Node(value)
    node.next = this.head
    this.head = node
    if (!this.tail) {
      this.tail = this.head
    }
  }
  
  hasCycle() {
    let fast = this.head
    let slow = this.head
    
    while (fast) {
      if (fast === slow) {
        return true
      }
      fast = fast.next.next
      slow = slow.next
    }
    return false
  }
  
  print() {
    let head = this.head
    while (head) {
      console.log(head.value)
      head = head.next
    }
  }
}

const ll = new LinkedList()
ll.push(1)
ll.push(2)
ll.push(3)
ll.push(4)
ll.push(5)
ll.tail.next = ll.head
ll.hasCycle()
```

## CCI 1.7
```javascript
const matrix = [
  ['a', 'b', 'c', 'd'],
  ['e', 'f', 'g', 'h'],
  ['i', 'j', 'k', 'l'],
  ['m', 'n', 'o', 'p'],
]

const rotateImage = matrix => {
  let n = matrix.length
  
  for (let layer = 0; layer < Math.floor(n / 2); layer++) {
    let first = layer
    let last = n - 1 - layer
    for (let i = first; i < last; i++) {
      let offset = i - first
      let top = matrix[first][i]
      matrix[first][i] = matrix[last - offset][first]
      matrix[last - offset][first] = matrix[last][last - offset]
      matrix[last][last - offset] = matrix[i][last]
      matrix[i][last] = top
    }
  }
  
  return matrix
}

rotateImage(matrix)
```

## Heap
```javascript
class Heap {
  constructor(elements, sort = ((a, b) => { return a < b })) {
    this._elements = elements
    this._sort = sort
    this._heapify()
  }
  
  _heapify() {
    for (let i = Math.floor(this._elements.length / 2) - 1; 0 <= i; i--) {
      this._siftDown(i);
    }
  }
  
  _siftUp(index) {
    let childIndex = index
    let parentIndex = this._parentIndex(childIndex)
    
    while (childIndex > 0 && 
           this._sort(this._elements[childIndex], this._elements[parentIndex])) {
      let temp = this._elements[childIndex]
      this._elements[childIndex] = this._elements[parentIndex]
      this._elements[parentIndex] = temp
      
      childIndex = parentIndex
      parentIndex = this._parentIndex(childIndex)
    }
    
  }
  
  _siftDown(index) {
    let parentIndex = index
    while (true) {
      let leftIndex = this._leftChildIndex(parentIndex)
      let rightIndex = this._rightChildIndex(parentIndex)
      let candidate = parentIndex
      
      if (leftIndex < this._elements.length && 
          this._sort(this._elements[leftIndex], this._elements[candidate])) {
        candidate = leftIndex
      }
            
      if (rightIndex < this._elements.length && 
          this._sort(this._elements[rightIndex], this._elements[candidate])) {
        candidate = rightIndex
      }
      
      if (parentIndex === candidate) {
        return
      }
      
      let temp = this._elements[parentIndex]
      this._elements[parentIndex] = this._elements[candidate]
      this._elements[candidate] = temp
      
      parentIndex = candidate
    }
  }
  
  _leftChildIndex(parentIndex) {
    return 2 * parentIndex + 1
  }
  
  _rightChildIndex(parentIndex) {
    return 2 * parentIndex + 2
  }
  
  _parentIndex(childIndex) {
    return Math.floor((childIndex - 1) / 2)
  }
  
  insert(element) {
    this._elements.push(element)
    this._siftUp(this._elements.length - 1)
  }
  
  remove() {
    if (this._elements.length < 1) {
      return null
    }
    
    let temp = this._elements[0]
    this._elements[0] = this._elements[this._elements.length - 1]
    this._elements[this._elements.length - 1] = temp
    
    let element = this._elements.pop()
    this._siftDown(0)
    return element
  }
}


const heap = new Heap([], (a, b) => { return a > b })
heap
heap.insert(5)
heap.insert(9)
heap.insert(2)
heap.insert(100)
heap
heap.remove()
heap.remove()
heap.remove()
heap.remove()
heap.remove()

```

## Heap Sort
```javascript
const a = [5, 3, 7, 6, 8, 7, 3, 2, 1, 1, 3]


const leftChildIndex = parentIndex => {
  return 2 * parentIndex + 1
}

const rightChildIndex = parentIndex => {
  return 2 * parentIndex + 2
}

const siftDown = (from, to, elements) => {
  let parent = from
  while (true) {
    let left = leftChildIndex(parent)
    let right = rightChildIndex(parent)
    let candidate = parent
    
    if (left < to && elements[left] > elements[candidate]) {
      candidate = left
    }
      
    if (right < to && elements[right] > elements[candidate]) {
      candidate = right
    }
      
    if (parent === candidate) return
    
    let temp = elements[parent]
    elements[parent] = elements[candidate]
    elements[candidate] = temp
    
    parent = candidate
  }
}

const heapify = elements => {
  for (let i = Math.floor(elements.length / 2) - 1; 0 <= i; i--) {
    siftDown(i, elements.length, elements)
  }
}

const heapSort = elements => {
  if (elements.length < 2) return elements
  
  heapify(elements)
  
  for (let i = (elements.length - 1); 0 <= i; i--) {
    let temp = elements[i]
    elements[i] = elements[0]
    elements[0] = temp
    
    siftDown(0, i, elements)
  }
}


heapSort(a)
a
```

## Radix Sort
```javascript
const radixSort = arr => {
  let done = false
  let digits = 1
  const radix = 10
  
  while (!done) {
    done = true
    const buckets = Array(radix).fill(null).map(() => [])
        
    for (let num of arr) {
      const remainingNum = Math.floor(num / digits)
      const digit = remainingNum % radix
      buckets[digit].push(num)
      
      if (remainingNum > 0) done = false
    } 
    
    arr = buckets.flat()
    digits *= 10
  }
  return arr
}

const arr = [109, 3, 55, 4, 6, 5, 41, 2, 66, 7, 6, 4, 322, 2, 1]
console.log(radixSort(arr))
```

## Trie
```javascript
class TrieNode {
  children = {}
  isTerminating = false
  constructor(key = null, parent = null) {
    this.key = key
    this.parent = parent  
  }
}

class Trie {
  constructor() {
    this.root = new TrieNode()
  }
  
  insert(collection) {
    if (collection.length < 1) return
    
    let current = this.root
    for (let element of collection) {
      if (!current.children[element]) {
        current.children[element] = new TrieNode(element, current)  
      }
      current = current.children[element]
    }
    current.isTerminating = true
  }
  
  remove(collection) {
    if (collection.length < 1) return
    
    let current = this.root
    for (let element of collection) {
      if (!current.children[element]) return
      current = current.children[element]
    }
    
    if (!current.isTerminating) return
    current.isTerminating = false
    
    while (current.parent && 
           !current.isTerminating && 
           Object.entries(current.children).length === 0 && 
           current.children.constructor === Object) {
      let parent = current.parent
      delete parent.children[current.key]
      current = parent
      parent = current.parent
    }
    
  }
  
  contains(collection) {
    if (collection.length < 1) return
    
    let current = this.root
    for (let element of collection) {
      if (!current.children[element]) return false
      current = current.children[element]
    }
    return current.isTerminating
  }
  
  collections(prefix) {
    if (prefix.length < 1) return
    
    let current = this.root
    for (let element of prefix) {
      if (!current.children[element]) return []
      current = current.children[element]
    }
    
    return this._collections(prefix, current)
  }
  
  _collections(prefix, current) {
    let collections = [];

    if (current.isTerminating) {
      collections.push(prefix)
    }
    
    for (let child of Object.values(current.children)) {
      let matches = this._collections(prefix + child.key, child)
      collections = [...collections, ...matches]
    }
    
    return collections
  }
} 

const t = new Trie()
t.insert("Hello")
t.insert("He")
t.collections("H")
```

## Minimal Tree / ListsFromTree
```javascript
const array = [1, 2, 3, 4, 5, 6, 7, 8, 9]

class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value
    this.left = left
    this.right = right
  }
}

const minimalTree = array => {
  if (array.length < 1) return
  const middleIndex = Math.floor(array.length / 2)
  const middleValue = array[middleIndex]
  const left = minimalTree(array.slice(0, middleIndex))
  const right = minimalTree(array.slice(middleIndex + 1))
  return new TreeNode(middleValue, left, right)
}

const listsFromTree = tree => {
  const lists = []
  _listsFromTree(lists, tree)
  return lists
}

const _listsFromTree = (lists, node, level = 0) => {
  if (!node) return
  if (lists[level]) {
    lists[level].push(node.value)
  } else {
    lists[level] = [node.value]
  }
  _listsFromTree(lists, node.left, level + 1)
  _listsFromTree(lists, node.right, level + 1)
}

const tree = minimalTree(array)
const lists = listsFromTree(tree)
console.log(lists)

```

## Rotate Matrix
```javascript
const matrix = [
  [1,  2,  3,  4],
  [5,  6,  7,  8],
  [9,  10, 11, 12],
  [13, 14, 15, 16]
]

const rotateMatrix = matrix => {
  if (matrix.length == 0) return
  let n = matrix.length
  let endIndex = n - 1
  
  for (let i = 0; i < (Math.floor(n / 2)); i++) {
    for (let j = i; j < endIndex - i; j++) {
      let temp
      
      temp = matrix[i][j]
      matrix[i][j] = matrix[j][endIndex - i]
      matrix[j][endIndex - i] = temp
      
      temp = matrix[j][endIndex - i]
      matrix[j][endIndex - i] = matrix[endIndex - i][endIndex - j]
      matrix[endIndex - i][endIndex - j] = temp

      temp = matrix[endIndex - i][endIndex - j]
      matrix[endIndex - i][endIndex - j] = matrix[endIndex - j][i]
      matrix[endIndex - j][i]  = temp
    }
  }
  
  return matrix 
}

rotateMatrix(matrix)
```

## Zero Matrix
```javascript
const matrix = [
  [1, 1, 1, 1, 1, 1, 1, 1, 1],
  [1, 1, 1, 1, 1, 1, 0, 1, 1],
  [1, 1, 0, 1, 1, 1, 1, 1, 1],
  [1, 1, 1, 1, 1, 1, 1, 1, 1]
]

const zeroMatrix = matrix => {
  if (matrix.length === 0) return matrix
  const m = matrix.length
  const n = matrix[0].length
  
  const validZeros = {}
  
  for (let rowIndex = 0; rowIndex < m; rowIndex++) {
    for (let columnIndex = 0; columnIndex < n; columnIndex++) {
      if (matrix[rowIndex][columnIndex] === 0) {
        validZeros[rowIndex] = columnIndex
      }
    } 
  }  
  
  for (let key of Object.keys(validZeros)) {
    let rowIndex = key
    let columnIndex = validZeros[key]
    
    for (let i = 0; i < n; i++) {
      matrix[rowIndex][i] = 0
    }
    
    for (let i = 0; i < m; i++) {
      matrix[i][columnIndex] = 0
    }
  }
  
  return matrix
}

zeroMatrix(matrix)
```

## Maze Generator
```javascript
const getNextCoordinate = (direction, [x, y]) => {
  switch (direction) {
      case 'n':
        return [x + 1, y]
      case 's':
        return [x - 1, y]
      case 'e':
        return [x, y + 1]
      case 'w':
        return [x, y - 1]
    }    
}

const isValidCoordinate = (maze, [x, y]) => {
  if (x < 0 || x >= maze.length) {
    return false
  }
  
  if (y < 0 || y >= maze.length) {
    return false
  }
  return true
}

const updateMaze = (maze, direction, current, next) => {
  switch (direction) {
    case 'n':
      current.n = false
      next.s = false
      break
    case 's':
      current.s = false
      next.n = false
      break
    case 'e':
      current.e = false
      next.w = false
      break
    case 'w':
      current.w = false
      next.e = false
      break
  } 
}

const generateMaze = (maze, [xStart, yStart]) => {
  const current = maze[xStart][yStart]
  current.visited = true
  
  randomizeDirection().forEach(direction => {
    const [x, y] = getNextCoordinate(direction, [xStart, yStart])
    if (!isValidCoordinate(maze, [x, y])) return;
    
    const next = maze[x][y];
    if (next.visited) return;
    
    updateMaze(maze, direction, current, next)
    generateMaze(maze, [x, y])
  })

  return maze;
};

```

## isValid
```javascript
const isValid = (tree, range = [Number.MIN_SAFE_INTEGER, Number.MAX_SAFE_INTEGER]) => {
  if (!tree) return true
  const [min, max] = range
  if (!(tree.value >= min) && !(tree.value <= max)) return false
  
  let leftBalanced = true
  let rightBalanced = true

  if (tree.left) {
    leftBalanced = isValid(tree.left, [min, tree.left.value])
  }
  
  if (tree.right) {
    rightBalanced = isValid(tree.right, [min, tree.right.value])
  }
  
  return leftBalanced && rightBalanced
}
```

## Project Dependencies
```javascript
const makeGraph = dependencies => {
  return dependencies.reduce((graph, [project, dependency]) => {
    if (graph[dependency]) {
      graph[dependency].push(project)
    } else {
      graph[dependency] = [project] 
    } 
    return graph
  }, {})
}

const getOrder = (projects, graph) => {
  let buildOrder = new Map()
  for (let project of projects) {
    let visiting = new Set()
    build(project, graph, buildOrder, visiting)  
  }
  return Object.keys(buildOrder)
}

const build = (project, graph, buildOrder, visiting) => {  
  if (!project) return
  if (visiting.has(project)) throw new Error('Cycle')
  if (buildOrder[project]) return
  
  visiting.add(project)
  
  let children = graph[project]
  if (children) {
    for (let child of children) {
        build(child, graph, buildOrder, visiting)  
    }    
  }
  
  visiting.delete(project)
  buildOrder[project] = true
  return

}

const buildOrder = (projects, dependencies) => {
  let graph = makeGraph(dependencies)
  return getOrder(projects, graph)
} 

buildOrder(['a', 'b', 'c', 'd', 'e', 'f'], 
           [['a', 'd'], ['f', 'b'], ['b', 'd'], ['f', 'a'], ['d', 'c']])
```

## 235. Lowest Common Ancestor of a Binary Search Tree
```javascript
var lowestCommonAncestor = function(root, p, q) {
    if (!root) return null
    if (root === p || root === q) return root
    
    const left = lowestCommonAncestor(root.left, p, q)
    const right = lowestCommonAncestor(root.right, p, q)
    
    if (left && right) return root
    if (!left && !right) return null
    return left ? left : right
};

var lowestCommonAncestor = function(root, p, q) {
    if (!root) return null
    
    if (root.val < p.val && root.val < q.val) {
        return lowestCommonAncestor(root.right, p, q)
    } else if (root.val > p.val && root.val > q.val) {
        return lowestCommonAncestor(root.left, p, q)
    } else {
        return root
    }
    return null
};
```

## Check BST 1
```javascript
const array = [1, 2, 3, 4, 5, 6, 7, 8, 9]

class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value
    this.left = left
    this.right = right
  }
}

const treeAsString = (tree, result = '') => {
  if (!tree) {
    result += 'X'
    return
  }
  result += `${tree.value}`
  result += treeAsString(tree.left)
  result += treeAsString(tree.right)
  return result
}

const checkBST = (tree, subtree) => {
  return treeAsString(tree).includes(treeAsString(subtree))
}

const minimalTree = array => {
  if (array.length < 1) return
  const middleIndex = Math.floor(array.length / 2)
  const middleValue = array[middleIndex]
  const left = minimalTree(array.slice(0, middleIndex))
  const right = minimalTree(array.slice(middleIndex + 1))
  return new TreeNode(middleValue, left, right)
}

const tree = minimalTree(array)
checkBST(tree, tree.left.left)
```

## Check BST 2
```javascript
const array = [1, 2, 3, 4, 5, 6, 7, 8, 9]

class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value
    this.left = left
    this.right = right
  }
}

const matchTree = (tree1, tree2) => {
  if (!tree1 && !tree2) return true  
  if (!tree1 || !tree2) return false
  if (tree1.value !== tree2.value) return false
  return matchTree(tree1.left, tree2.left) || matchTree(tree1.right, tree2.right) 
}

const checkBST = (tree, subtree) => {
  if (!tree) return false
  if (!subtree) return true
  if (tree.value === subtree.value && matchTree(tree, subtree)) {
    return true
  }
  return checkBST(tree.left, subtree) || checkBST(tree.right, subtree)
}

const minimalTree = array => {
  if (array.length < 1) return
  const middleIndex = Math.floor(array.length / 2)
  const middleValue = array[middleIndex]
  const left = minimalTree(array.slice(0, middleIndex))
  const right = minimalTree(array.slice(middleIndex + 1))
  return new TreeNode(middleValue, left, right)
}

const tree = minimalTree(array)
checkBST(tree, tree.left.left)
```

## Random Node
```javascript
class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value
    this.left = left
    this.right = right
    this.children = 0
  }
  
  min() {
    if (this.left) {
      return this.left.min()
    } else {
      return this.value
    }
  }
}

class Tree {
  constructor() {
    this.root = null
  }

  insert(value) {
    this.root = this._insert(value, this.root)
  }
  
  _insert(value, node) {    
    if (!node) {
      return new TreeNode(value)
    }
    
    node.children++
    
    if (value < node.value) {
      node.left = this._insert(value, node.left)
    } else {
      node.right = this._insert(value, node.right)
    }
    return node
  }
  
  remove(value) {
    this.root = this._remove(value, this.root)
  }
  
  _remove(value, node) {
    if (!node) {
      return null
    }
    
    node.children--
    
    if (value === node.value) {
      if (!node.left && !node.right) {
        return null
      }
            
      if (!node.left) {
        return node.right
      }
      
      if (!node.right) {
        return node.left
      }
      
      node.value = node.right.min()
      this._remove(node.value, node.right)
      
    } else if (value < node.value) {
      node.left = this._remove(value, node.left)     
    } else {
      node.right = this._remove(value, node.right)
    }
    return node
  }
  
  find(value) {
    let current = this.root
    
    while (current) {
      if (value === current.value) {
        return true
      }
      
      if (value < current.value) {
        current = current.left
      } else {
        current = current.right
      }
    }
    return false
  }
  
  getRandom() {
    if (!this.root) return
    
    const index = Math.floor(Math.random() * this.root.children + 1)
    console.log("index", index)
    return this.getIthNode(index, this.root)
  }
  
  getIthNode(i, node) {
    let leftSize = node.left ? node.left.children + 1 : 0

    if (i < leftSize) {
      return this.getIthNode(i, node.left)
    } else if (i === leftSize) {
      return node
    } else {
      return this.getIthNode((i - (leftSize + 1)), node.right)
    }
  }
}

const tree = new Tree()
tree.insert(5)
tree.insert(3)
tree.insert(1)
tree.insert(7)
tree.insert(6)
tree.insert(9)
tree.getRandom().value

```

## Max Depth of Binary Tree
```javascript
const array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16]

class TreeNode {
  constructor(value, left = null, right = null) {
    this.value = value
    this.left = left
    this.right = right
  }
}

const maxDepth = (tree, depth = 0) => {
  if (!tree) return depth - 1
  
   return Math.max(maxDepth(tree.left, depth + 1), 
                  maxDepth(tree.right, depth + 1))
}

const minimalTree = array => {
  if (array.length < 1) return
  const middleIndex = Math.floor(array.length / 2)
  const middleValue = array[middleIndex]
  const left = minimalTree(array.slice(0, middleIndex))
  const right = minimalTree(array.slice(middleIndex + 1))
  return new TreeNode(middleValue, left, right)
}

const tree = minimalTree(array)
tree.right.left
maxDepth(tree)
```

## Flip Bit
```javascript
const flipBit = bin => {
  if (!bin) return 1
  
  let max = 0
  let current = 0
  let previous = 0
  
  while (bin) {
    if (bin & 1) {
      current++
    } else {
      previous = current
      current = 0
    }
    max = Math.max(current + previous + 1, max)
    bin >>>= 1
  }
  return max
}

flipBit(0b11011101111)
```

## longest ones
```javascript
const longest1s = bin => {
  let count = 0
  
  while (bin) {
    bin &= bin << 1
    count++
  }
  return count
}

longest1s(14)
```

## First Set Bit
```javascript
const firstSetBit = bin => {
  if (!bin) return null
  
  let currentIndex = 0
  
  while (bin) {
    if (bin & 1) {
      return currentIndex
    }
    
    bin >>>= 1
    currentIndex++
  }
  return null
}

firstSetBit(0b11001111000)
```

## Insertion
```javascript
function dec2bin(dec){
    return (dec >>> 0).toString(2);
}

const insertion = (n, m, i, j) => {
  const left = ~0 << j + 1
  const right = (1 << i) - 1
  const mask = left | right 
  const nCleared = n & mask
  const mShifted = m << i
  return nCleared | mShifted
}

const t = insertion(0b10000000000, 0b10011, 2, 6)
console.log(dec2bin(t))
```

## Flip Bit at K
```javascript
const decToBin = num => {
  return (num >>> 0).toString(2)
}

const flipK = (bin, k) => {
  let mask = 1 << k
  return bin ^ mask
}

const t = flipK(0b11011, 2)
decToBin(t)
```

## Power of 2
```javascript
const powerOf2 = bin => {
  return (bin & bin - 1) === 0
}

powerOf2(1)
```

## CCI 5.6
```javascript
const decToBin = bin => {
  return (bin >>> 0).toString(2)
}

const conversion = (bin1, bin2) => {
  let diff = bin1 ^ bin2
  let count = 0
  
  while (diff) {
    count++
    diff = diff & (diff - 1)
  }
  return count
}

conversion(29, 15)
```

## Right Most Diff
```javascript
const decToBin = bin => {
  return (bin >>> 0).toString(2)
}

const rightMostDiff = (bin1, bin2) => {
  let diff = bin1 ^ bin2
  let position = 0
  while (diff) {
    position++
    if (diff & 1) return position
    diff >>>= 1
  }
  return null
}

rightMostDiff(11, 9)
```

## Toggle In Range
```javascript
const decToBin = bin => (bin >>> 0).toString(2);

const toggleInRange = (bin, r, l) => {
  r--
  l--
  
  let mask = ((1 << l) - 1) << r
  return bin ^ mask
}
  
toggleInRange(17, 2, 3)
```

## Rotate bits

```javascript
const decToBin = bin => {
  return (bin >>> 0).toString(2)
}

const uIntN = (bin, n) => {
  let upperBound = Math.pow(2, n) - 1
  return bin & upperBound
}

const uInt16 = (bin) => uIntN(bin, 16)

const rotateBits = (bin, rotations) => {
  let left = uInt16(bin << rotations) | uInt16(bin >> 16 - rotations)
  let right = uInt16(bin >> rotations) | uInt16(bin << 16 - rotations)
  return [left, right]
}

rotateBits(229, 3)
```

## Pairwise Swap
```javascript
const decToBin = bin => {
  return (bin >>> 0).toString(2)
}

const pairwiseSwap = bin => {
  return ((bin & 0xaaaaaaaa) >> 1) | ((bin & 0x55555555) << 1)
}

const t = pairwiseSwap(0b11011010)
console.log(decToBin(t))
```

## Number is Sparse
```javascript
const decToBin = bin => {
  return (bin >>> 0).toString(2)
}

const isSparse = num => {
  let num_shifted = num >>> 1
  return (num & num_shifted) == 0
}

isSparse(2)
isSparse(3)

```

## Min Stack
```javascript
class Stack {
  constructor() {
    this.storage = []
    this.min = []
  }
  
  push(value) {
    let min = this.min[this.min.length - 1]
    if (min === undefined || value <= min) {
      this.min.push(value)
    }
    this.storage.push(value)
  }
  
  pop() {
    let min = this.min[this.min.length - 1]
    if (min === undefined) return
    
    let value = this.storage[this.storage.length - 1]
    if (value === min) {
      this.min.pop(value)
    }
    return this.storage.pop()
  }
  
  minValue() {
   return this.min[this.min.length - 1]
  }
}

const s = new Stack()
s.push(9)
s.push(2)
s.push(4)
s.push(1)
s.push(0)
s.push(3)
s.push(6)
s.push(0)
s
s.pop()
s
s.pop()
s
s.pop()
s
s.pop()
s
s.pop()
s
s.minValue()
```

## Stack of Plates
```javascript
class Stack {
  constructor(capacity) {
    this.capacity = capacity
    this.storage = [[]]
  }
  
  currentStack() {
    return this.storage[this.storage.length - 1]
  }
  
  push(value) {
    let currentStack = this.currentStack()
    if (currentStack.length >= this.capacity - 1) {
      this.storage.push([value])
    }
    
    currentStack.push(value)
  }
    
  pop() {
    let currentStack = this.currentStack()
    if (!currentStack) return 
    
    if (currentStack.length === 0) {
      this.storage.pop()
      return this.pop()
    }
    
    return currentStack.pop()
  }
  
  popAt(index) {
    if (index >= this.storage.length) return null
    
    let stack = this.storage[index]
    return stack.pop()
  }
  
}

const stack = new Stack(3)
stack
stack.push(1)
stack.push(2)
stack.push(3)
stack.push(4)
stack.push(5)
stack.push(6)
stack
stack.popAt(6)
stack.popAt(1)
stack
stack.popAt(1)
stack
stack.popAt(1)
stack
stack.popAt(1)
stack
stack.popAt(0)
stack
stack.popAt(0)
stack
stack.push(6)
stack
stack.pop()
stack
stack.pop()
stack
stack.pop()
stack
stack.pop()
stack
stack.pop()
stack
stack.pop()
stack
stack.pop()
stack
stack.pop()
stack
stack.pop()
stack
stack.pop()
stack
stack.pop()
stack
stack.pop()
stack
stack.pop()
stack
```

## Sort Stack
```javascript
class Stack {
  constructor() {
    this.storage = []
  }
  
  push(value) {
    this.storage.push(value)
  }
  
  pop() {
    return this.storage.pop()
  }
  
  peek() {
    return this.storage[this.storage.length - 1]
  }
  
  isEmpty() {
    return this.storage.length === 0
  }
  
  sort() {
    let buffer = new Stack()
    
    while (!this.isEmpty()) {
      let temp = this.storage.pop()
      
      while (buffer.peek() > temp) {
        this.storage.push(buffer.pop()) 
      }
      buffer.push(temp)
    }
        
    while (!buffer.isEmpty()) {
      this.storage.push(buffer.pop())
    }
  }
}

const stack = new Stack()
stack.push(9)
stack.push(2)
stack.push(4)
stack.push(12)
stack.push(1)
stack.push(3)
stack
stack.sort()
stack
```

## Animal Shelter
```javascript
class Animal {
  constructor(name) {
    this.name = name
    this.age = null
  }
}

class Dog extends Animal {}
class Cat extends Animal {}

class AnimalQueue {
  constructor() {
    this.dogQueue = []
    this.catQueue = []
  }
  
  enqueue(animal) {
    animal.age = Date()
    
    if (animal instanceof Dog) {
      this.dogQueue.push(animal)
      return
    }
    
    if (animal instanceof Cat) {
      this.catQueue.push(animal)
      return
    }
  }
  
  dequeueAny() {
    if (!this.dogQueue.length && !this.catQueue.length) return null
    if (!this.dogQueue.length) return this.catQueue.shift()
    if (!this.catQueue.length) return this.dogQueue.shift()
    
    let nextDog = this.dogQueue[this.dogQueue.length - 1]
    let nextCat = this.catQueue[this.catQueue.length - 1]
    
    if (nextDog > nextCat) {
      return this.dogQueue.shift()
    } else {
      return this.catQueue.shift()
    }
  }
  
  dequeueDog() {
    return this.dogQueue.shift()
  }
  
  dequeueCat() {
    return this.catQueue.shift()
  }
  
}


const queue = new AnimalQueue()
queue.enqueue(new Dog("Dog1"))
queue.enqueue(new Cat("Cat1"))
queue.enqueue(new Cat("Cat2"))
queue.enqueue(new Cat("Cat3"))
queue.enqueue(new Cat("Cat4"))
queue.enqueue(new Dog("Dog2"))
queue.enqueue(new Dog("Dog3"))
queue.dequeueCat().name
queue
queue.dequeueDog().name
queue
queue.dequeueAny().name
queue.dequeueDog().name
queue.dequeueAny().name
queue.dequeueCat().name
queue.dequeueAny().name

```

## Ring Buffer
```javascript
class RingBuffer {
  constructor(size) {
    this.storage = Array(size).fill(null)
    this.readIndex = 0
    this.writeIndex = 0
  }
  
  write(value) {
    if (this.isFull()) {
      return false
    }
    
    this.storage[this.writeIndex % this.count()] = value
    this.writeIndex++
    return true 
  }
  
  read() {
    if (this.isEmpty()) {
      return null
    }
    
    let element = this.storage[this.readIndex % this.count()]
    this.readIndex++
    return element
  }
  
  isFull() {
    return this.avaliableSpaceForWriting() === 0
  }
  
  isEmpty() {
    return this.avaliableSpaceForReading() === 0
  }
  
  avaliableSpaceForWriting() {
    return this.count() - this.avaliableSpaceForReading()
  }
  
  avaliableSpaceForReading() {
    return this.writeIndex - this.readIndex
  }
  
  count() {
    return this.storage.length
  }
}

const ringBuffer = new RingBuffer(4)
ringBuffer.write(1)
ringBuffer.read()
ringBuffer.write(2)
ringBuffer.write(3)
ringBuffer.read()
ringBuffer.read()
ringBuffer.read()
ringBuffer.write(4)
ringBuffer.write(5)
ringBuffer.write(5)
ringBuffer.write(6)
ringBuffer.read()
ringBuffer.write(7)
ringBuffer.write(8)
ringBuffer.read()
ringBuffer
```

## Parenthesis Matching
```javascript
const parenPosition = (string, openPosition) => {
  
  let position = openPosition + 1
  let nestDepth = 0
  
  while (position < string.length) {
    let char = string[position]
    
    if (char === '(') {
      nestDepth++
    }
    
    if (char === ')') {
      if (nestDepth === 0) return position
      nestDepth--
    } 
    
    position++
  }
  
}

parenPosition("Sometimes (when I nest them (my parentheticals) too much (like this (and this))) they get confusing.", 10)
```

## Bracket Validator
```javascript
const bracketValidator = string => {
  const openers = new Set(['(', '{', '['])
  const closers = new Set([')', '}', ']'])
  const map = { '(': ')', '{': '}', '[': ']' }
  const seen = []
  
  for (let char of string) {
    if (openers.has(char)) {
      seen.push(char)
      continue
    }
    
    if (closers.has(char)) {
      let compliment = map[seen[seen.length - 1]]
      if (compliment !== char) {
        return false
      }
      seen.pop()
    }
  }
  return seen.length === 0
}

bracketValidator("{ [ ] ( ) }") // true
bracketValidator("{ [ ( ] ) }") // false
bracketValidator("{ [ }") // false
```

## Next Larger Element
```javascript
const nextLargerElement = elements => {
  let result = []
  let stack = []
  
  for (let element of elements) {
    if (stack.length === 0 || stack[stack.length - 1] >= element) {
      stack.push(element)
      continue
    }
    
    while (true) {
      if (stack[stack.length - 1] < element) {
          stack.pop()
          result.push(element)
          continue
      }
      break
    }
    stack.push(element)
  }
  
  while (stack.length !== 0) {
    stack.pop()
    result.push(-1)
  }
  return result
}

nextLargerElement([4, 3, 2, 1])



// or with queueconst nextLarger = elements => {
  const queue = []
  const result = []
  
  for (let element of elements) {    
    if (queue[0] > element) {
      queue.push(element)
      continue
    }
    
    while (queue.length && queue[0] < element) {
      result.push(element)
      queue.shift()
    }
    queue.push(element)    
  }
  
  while (queue.length) {
    result.push(-1)
    queue.shift()
  }
  
  return result
}

nextLarger([1, 3, 2, 4])

```

## Next Larger - O(1) Space
```javascript
const nextLarger = nums => {
  let queueIndex = 0
  let currentIndex = 0
  
  while (currentIndex < nums.length) {
    while (nums[currentIndex] >= nums[queueIndex] && queueIndex < nums.length) {
      queueIndex++
    }
    
    if (queueIndex >= nums.length) {
      nums[currentIndex] = -1
    } else {
      nums[currentIndex] = nums[queueIndex]
    }
    currentIndex++
  }
}

const t = [5, 3, 2, 10, 6, 8, 1, 4, 12, 7, 4]
nextLarger(t)
console.log(t)
```

## Stack with Two Queues
```javascript
class Stack {
  constructor() {
    this.left = []
    this.right = []
  }
  
  push(value) {
    this.left.push(value)
  }
  
  pop() {
    while (this.left.length > 1) {
      this.right.push(this.left.shift())
    }
    
    let element = this.left.shift()
    
    let temp = this.left
    this.left = this.right
    this.right = temp
    
    return element
  }

  count() {
    return this.left.length + this.right.length
  }
  
  isEmpty() {
    return this.left.length === 0 && this.right.length === 0
  }
}

const stack = new Stack()
stack.push(1)
stack.push(2)
stack.pop()
stack.push(3)
stack.pop()
stack.pop()
stack.push(4)
stack.push(5)
stack.pop()
stack.pop()
stack.pop()
```

## Circular Tour
```javascript
const circularTour = (stops, size) => {
  let start = 0
  let end = 1
  let gas = stops[start][0] - stops[start][1]
  
  while (end !== start || gas < 0) {
    while (gas < 0 && start !== end) {
      gas -= stops[start][0] - stops[start][1]
      start = (start + 1) % size
      
      if (start === 0) return -1
    }

    gas += stops[end][0] - stops[end][1]
    end = (end + 1) % size
  }
  return start
}

circularTour([[4, 6], [6, 5], [7, 3], [4, 5]], 4)
```

## Non Repeats
```javascript
const nonRepeating = stream => {
  const counts = {}
  const queue = []
  return stream.reduce((result, element) => {
    queue.push(element)
    counts[element] = counts[element] + 1 || 1
    
    while (queue.length) {
      if (counts[queue[0]] <= 1) break
      queue.shift()
    }

    if (queue.length) {
      result.push(queue[0])
      return result
    }
    
    result.push("-1")
    return result
  }, [])
}

nonRepeating(['a', 'a', 'a', 'b', 'c', 'a', 'b', 'd', 'g', 'c'])
nonRepeating(['A', 'Q', 'I', 'Z', 'Q', 'A', 'Z', 'P', 'N'])
```

## Is K Bit Set
```javascript
const decToBin = bin => {
  return (bin >>> 0).toString(2)
}

const kSet = (bin, k) => {
  let mask = 1 << k
  return (bin & mask) !== 0
}

kSet(0b110110, 3)
```

## Flip K
```javascript
const decToBin = bin => {
  return (bin >>> 0).toString(2)
}

const flipK = (bin, k) => {
  let mask = 1 << k
  return (bin ^ mask)
}

const num = flipK(0b110110, 0)
console.log(decToBin(num))

```

## isEven
```javascript
const isEven = num => (num & 1) === 0
isEven(4)
```

## Clear Through I
```javascript
const decToBin = num => {
  return (num >>> 0).toString(2)
}

const clearThroughI = (bin, i) => {
  let mask = (1 << i) - 1
  return bin & mask
}

const num = clearThroughI(0b1100101, 4)
console.log(decToBin(num))
```

## Update I
```javascript
const decToBin = num => {
  return (num >>> 0).toString(2)
}

const updateI = (bin, i, value) => {
  let mask = ~(1 << i)
  return (bin & mask) | (value << i)
}

const num = updateI(0b1100101, 1, 1)
console.log(decToBin(num))
```

## Stolen Breakfast Drone
```javascript
const uniqueElement = elements => {
  let result
  
  for (let element of elements) {
    result ^= element
  }
  
  return result
}

uniqueElement([5, 5, 2, 3, 1, 2, 6, 3, 1])
```

## Twos Complement
```javascript
const decToBin = num => {
  return (num >>> 0).toString(2)
}

const twosComplement = bin => {
  return (~bin) + 1
}

const num = twosComplement(5)
decToBin(num)
```

## Set K
```javascript
const setK = (bin, k) => bin | (1 << k)
setK(15, 3)
```

## Rotate List
```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
  }
}

const node1 = new Node(2)
const node2 = new Node(4)
const node3 = new Node(7)
const node4 = new Node(8)
const node5 = new Node(9)
// const node6 = new Node(6)
// const node7 = new Node(7)
// const node8 = new Node(8)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
// node5.next = node6
// node6.next = node7
// node7.next = node8

const print = list => {
  let current = list
  
  while (current) {
    console.log(current.value)
    current = current.next
  }
}

const rotateList = (head, rotations) => {
  let newTail = head
  rotations = rotations - 1
  
  while (newTail.next && rotations) {
    rotations--
    newTail = newTail.next
  }
  
  let newHead = newTail.next
  newTail.next = null
  
  let lastNode = newHead
  while (lastNode.next) {
    lastNode = lastNode.next
  }
  
  lastNode.next = head
  return newHead
}

const list = rotateList(node1, 3)
print(list)

```

## Reverse list given groups
```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
  }
}

const node1 = new Node(1)
const node2 = new Node(2)
const node3 = new Node(2)
const node4 = new Node(4)
const node5 = new Node(5)
const node6 = new Node(6)
const node7 = new Node(7)
const node8 = new Node(8)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7
node7.next = node8

const print = list => {
  let current = list
  
  while (current) {
    console.log(current.value)
    current = current.next
  }
}

const reverse = list => {
  let current = list
  let next = null
  let previous = null
  
  while (current) {
    next = current.next
    current.next = previous
    previous = current
    current = next
  }
  return previous
}

const nReverse = (list, n) => {
  if (!list) return null
  
  let head = list
  let position = 1
  
  while (list, position !== n) {
    list = list.next 
    position++
  }
  
  let nextList = list.next
  list.next = null
  
  let reversedList = reverse(head)
  let tail = head
  tail.next = nReverse(nextList, n)
  return reversedList
}

const list = nReverse(node1, 4)
print(list)

```

## Flatten Linked List
```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
    this.bottom = null
  }
}

const node5 = new Node(5)
const node7 = new Node(7)
const node8 = new Node(8)
const node30 = new Node(30)
const node10 = new Node(10)
const node20 = new Node(20)
const node19 = new Node(19)
const node22 = new Node(22)
const node50 = new Node(50)
const node28 = new Node(28)
const node35 = new Node(35)
const node40 = new Node(40)
const node45 = new Node(45)

node5.bottom = node7
node7.bottom = node8
node8.bottom = node30
node5.next = node10
node10.bottom = node20
node10.next = node19
node19.bottom = node22
node22.bottom = node50
node19.next = node28
node28.bottom = node35
node35.bottom = node40
node40.bottom = node45

const print = list => {
  let current = list
  
  while (current) {
    console.log(current.value)
    current = current.bottom
  }
}

const merge = (list1, list2) => {
  if (!list1 && !list2) return
  
  let dummyNode = new Node(-1)
  let current = dummyNode
  
  while (list1 && list2) {    
    if (list1.value > list2.value) {
      current.bottom = list2
      list2 = list2.bottom
    } else {
      current.bottom = list1
      list1 = list1.bottom
    }
    current = current.bottom
  }
  
  if (list1) {
    current.bottom = list1
  } else {
    current.bottom = list2
  }
  return dummyNode.bottom
}

const flatten = list => {
  if (!list) return null
  return merge(list, flatten(list.next))
}

const list = flatten(node5)
print(list)
```

## isPalindrome

```javascript
const isPalindrome = string => {
  if (!string.length) return false
  
  let left = 0
  let right = string.length - 1
  
  while (left < right) {
    if (string[left].toLowerCase() !== string[right].toLowerCase()) {
      return false 
    }
    
    left++
    right--
  }
  return true
}

isPalindrome("rotator")
isPalindrome("Rats live on no evil star")
isPalindrome("Never odd or even")
isPalindrome("Hello, world")
```

## Swift Code Challeng  3
```javascript
const counts = str => {
  return str
    .split('')
    .reduce((result, element) => {
      result[element] = 1 + (result[element] || 0)
      return result
  }, {})
}

const sameChars = (str1, str2) => {
  if (str1.length !== str2.length) return false
  
  const str1Counts = counts(str1)
  const str2Counts = counts(str2)
  
  for (let key in str1Counts) {
    if (!str2Counts[key]) return false
    if (str1Counts[key] !== str2Counts[key]) return false
  }
  
  return true
}

sameChars("abca", "abca") // true
sameChars("abc", "cba") // true
sameChars(" a1 b2 ", " b1 a2 ") // true
sameChars("abc", "abca") // false
sameChars("abc", "Abc") // false
sameChars("abc", "cbAa") // false
sameChars("abcc", "abca") // false
```

## Swift Code Challenge 5
```javascript
const charCount = (str, char) => {
  return str
    .split('')
    .reduce((result, element) => element == char ? result + 1 : result, 0)
}

charCount("The rain in Spain", "a") // 2
charCount("Mississippi", "i") // 4
charCount("Hacking with Swift", "i") // 3
```

## Swift Code Challenge 6
```javascript
const removeDups = str => {
  let seenChars = new Set()
  return str
    .split('')
    .filter(char => {
      if (seenChars.has(char)) return false
      seenChars.add(char)
      return true
    })
    .join('')
}

removeDups("wombat")
removeDups("hello")
removeDups("Mississippi")
```

## Unique string
```javascript
const isUnique = str => {
  for (let i in str) {
    for (let j in str) {
      if (i === j) continue
      if (str[i] === str[j]) return false
    }
  }
  return true
}

const isUnique2 = str => {
  const unique = new Set(str)
  return unique.size === str.length
}

const isUnique3 = str => {
  let checker = 0
  for (let i = 0; i < str.length; i++) {
    let value = str.charCodeAt(i) - 'a'.charCodeAt(0)
    if ((checker & (1 << value)) > 0) {
      return false
    }
    checker |= (1 << value)
  }
  return true
}

isUnique3("hello")
```

## isPalindrome
```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
  } 
}

let node1 = new Node('R')
let node2 = new Node('A')
let node3 = new Node('C')
let node4 = new Node('E')
let node5 = new Node('C')
let node6 = new Node('A')
let node7 = new Node('R')

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7

const print = list => {
  while (list) {
    console.log(list.value)
    list = list.next
  }
}

const reverse = list => {
  let current = list
  let previous = null
  let next = null
  
  while (current) {
    next = current.next
    current.next = previous
    previous = current
    current = next
  }
  return previous
}

const getMiddle = list => {
  let fast = list
  let slow = list
  
  while (fast && fast.next) {
    fast = fast.next.next
    slow = slow.next
  }
  return slow
}

const checkEqual = (list1, list2) => {
  let diff = 0
  
  while (list1 || list2) {
    let list1Value = list1 ? list1.value : null
    let list2Value = list2 ? list2.value : null
    
    if (list1Value !== list2Value) {
      diff++
    }    
    
    if (diff > 1) return false
    
    list1 = list1 ? list1.next : null
    list2 = list2 ? list2.next : null
  }
  return true
}

const isPalindrome = list => {
  if (!list) return false
  
  let head = list
  let middleNode = getMiddle(list) 
  
  while (list && list.next !== middleNode) {
    list = list.next
  }
  
  list.next = null
  let reversedList = reverse(middleNode)
  
  let areEqual = checkEqual(head, reversedList)
  list.next = reverse(reversedList)
  return areEqual
}

isPalindrome(node1)
```

## isPanagram
```javascript
const isPanagram = str => {
  const set = new Set(str.toLowerCase())
  const letters = [...set].filter(char => {
    return (char >= 'a' && char <= 'z')
  })
  return letters.length === 26
}

isPanagram('The quick brown fox jumps over the lazy dog')
isPanagram('The quick brown fox jumped over the lazy dog')
```

## Vowels and Consonants
```javascript
const vowelsAndConsonants = str => {
  const vowels = new Set(['A', 'E', 'I', 'O', 'U'])
  const consonants = new Set(['B', 'C', 'D', 'F', 'G', 'H', 'J', 'K', 'L', 'M', 
                              'N', 'P', 'Q', 'R', 'S', 'T', 'V', 'W', 'X', 'Y', 'Z'])

  return str.toUpperCase().split('').reduce((result, element) => {
    let [vowelsCount, consonantsCount] = result
    if (vowels.has(element)) vowelsCount++
    if (consonants.has(element)) consonantsCount++
    return [vowelsCount, consonantsCount]
  }, [0, 0])
}

vowelsAndConsonants('Swift Coding Challenges')
vowelsAndConsonants('Mississippi')
```

## Three Diff Letters
```javascript
const threeDiffLetters = (str1, str2) => {
  if (str1.length !== str2.length) return false
  
  let diffCount = 0
  
  for (let index in str1) {
    if (str1[index] !== str2[index]) diffCount++
    if (diffCount > 3) return false
  }
  return true
}

threeDiffLetters("Clamp", "Cramp") // true
threeDiffLetters("Clamp", "Crams") // true
threeDiffLetters("Clamp", "Grams") // true
threeDiffLetters("Clamp", "Grans") // false
threeDiffLetters("Clamp", "Clam") // false
threeDiffLetters("clamp", "maple") // false
```

## Longest Prefix
```javascript
const longestPrefix = str => {
  if (!str) return ''
  
  const words = str.split(' ')
  let longestPrefix = ''
           
  for (let char of words[0]) {
    let localPrefix = longestPrefix + char
    
    for (let word of words.slice(1)) {
      if (!word.startsWith(localPrefix)) return longestPrefix
    }
    
    longestPrefix = localPrefix
  }
  return longestPrefix
}

longestPrefix('swift switch swill swim') // swi
longestPrefix('flip flap flop') // fl
```

## String Permutations
```javascript
const stringPermutations = (str, other = '') => {
  if (!str.length) {
    console.log(other)
    return
  }
  
  for (var index = 0; index < str.length; index++) {
    let left = str.substring(0, index)
    let right = str.substring(index + 1)
    stringPermutations(left + right, other + str[index])
  }
}

stringPermutations('abc')
```

## FizzBuzz
```javascript
const divisible = (num, den) => {
  return num % den === 0
}

const translate = num => {
  if (divisible(num, 15)) return 'FizzBuzz'
  if (divisible(num, 3)) return 'Fizz'
  if (divisible(num, 5)) return 'Buzz'
  return num
}

const fizzBuzz = stop => {
  for (let i = 1; i <= stop; i++) {
    console.log(translate(i))
  }
}

fizzBuzz(100)
```

## Random num between two values
```javascript
const random = (min, max) => {
  return Math.floor(Math.random() * (max - min + 1)) + min
}

random(8, 10)
```

## 50. Pow(x, n)
```javascript
var myPow = function(x, n) {
  let result = 1  
  
  if (n < 0) {
      n = -n
      x = 1 / x
  }
  
  while (n) {
    if (n % 2) result *= x
    n = Math.floor(n / 2)
    x *= x
  }
    
  return result
};
```

## SumInString
```javascript
const sumInString = str => {
  let result = 0
  let currentNumber = ''
  
  for (let char of str) {
    if (!Number.isNaN(+char)) {
      currentNumber += char
      continue
    }
    result += (+currentNumber)
    currentNumber = ''
  }
  result += (+currentNumber)
  return result
}

sumInString('a1b2c3')
sumInString('a10b20c30')
sumInString('h8ers')
```

## sqrt
```javascript
const sqrt1 = num => {
  if (!num) return 0
  
  let lowerBound = 0
  let upperBound = 1 + Math.floor(num / 2)
  
  while (lowerBound < upperBound) {
    let midPoint = Math.floor((upperBound - lowerBound) / 2) + lowerBound
    let midSquared = midPoint * midPoint
    
    if (midSquared === num) {
      return midPoint
    }
    
    if (midSquared > num) {
      upperBound = midPoint - 1
    } else {
      lowerBound = midPoint + 1
    }
  }
  return lowerBound
}

const sqrt2 = num => {
  return Math.floor(Math.pow(num, 0.5))  
}

sqrt1(9)
sqrt1(16777216)
sqrt1(16)
sqrt1(15)


// • The number 9 should return 3.
// • The number 16777216 should return 4096. • The number 16 should return 4.
// • The number 15 should return 3.
```

## Count the numbers
```javascript
const countNums1 = (nums, num) => {
  return nums.map(ele => `${ele}`)
    .reduce((result, element) => result + element, '')
    .split('')
    .filter(element => element == num)
    .length
}

const countNums2 = (nums, numToCount) => {
  let result = 0
  
  for (let num of nums) {
    while (num) {
      if (num % 10 === numToCount) result++
      num = Math.floor(num / 10)
    }
  }
  return result
}

countNums2([5, 15, 55, 515], 5)
countNums2([5, 15, 55, 515], 1)
countNums2([55555], 5)
countNums2([55555], 1)
```

## First index of
```javascript
const firstIndexOf = (collection, element) => {
  for (let index in collection) {
    if (collection[index] === element) return +index
  }
  return null
}

firstIndexOf([1, 2, 3], 1)
firstIndexOf([1, 2, 3], 3)
firstIndexOf([1, 2, 3], 5)

```

## Map
```javascript
const map = (collection, transform) => {
  return collection.reduce((result, element) => {
    result.push(transform(element))
    return result
  }, [])
}

map([1, 2, 3, 4], num => num * num)
```

## Sum even repeats
```javascript
const counts = collection => {
  return collection.reduce((result, element) => {
    result[element] = 1 + (result[element] || 0)
    return result
  }, {})
}

const isEven = num => (num & 1) === 0

const sumEvenRepeats = (...nums) => {
  let numCounts = counts(nums)
  return Object.entries(numCounts).reduce((result, [key, value]) => {
    if (isEven(value)) result += (+key)
    return result
  }, 0)
}

sumEvenRepeats(1, 2, 2, 3, 3, 4)
```

## Partition
```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
  }
}

const node1 = new Node(3)
const node2 = new Node(5)
const node3 = new Node(8)
const node4 = new Node(5)
const node5 = new Node(10)
const node6 = new Node(2)
const node7 = new Node(1)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7

const print = list => {
  while (list) {
    console.log(list.value)
    list = list.next
  }
}

const partition = (list, p) => {
  if (!list || !list.next) return list
  
  let head = list
  let tail = list
  let current = list
  
  while (current) {
    const next = current.next
    
    if (current.value < p) {
      current.next = head
      head = current
    } else {
      tail.next = current
      tail = tail.next
    }
    
    current = next
  }
  tail.next = null
  return head
}

const l = partition(node1, 5)
print(l)
```

## Check Permutations
```javascript
const counts = str => {
  return str.split('').reduce((result, element) => {
    result[element] = 1 + (result[element] || 0)
    return result
  }, {})
}

const isPermutation = (str1, str2) => {
  if (str1.length !== str2.length) return false
  
  const str1Counts = counts(str1)
  
  for (let char of str2) {
    str1Counts[char]--
    if (!str1Counts[char] < 0) return false
  }
  console.log(str1Counts)
  
  return true
}

isPermutation("hello", 'lehfo')
```

## urlify
```javascript
const urlify = (str, length) => {
  const charArr = str.split('')
  let index = str.length - 1
  for (let i = length - 1; i >= 0; i--) {
    if (charArr[i] === ' ') {
      charArr[index] = '0'
      charArr[index - 1] = '2'
      charArr[index - 2] = '%'
      index -= 3
    } else {
      charArr[index--] = charArr[i]
    }
  }
  return charArr.join('')
}

urlify("Mr John Smith Too      ", 17)
```

## One Away
```javascript
const oneAway = (str1, str2) => {
  if (Math.abs(str1.length - str2.length) > 1) return false
  
  let long = str1.length > str2.length ? str1 : str2
  let short = str1.length > str2.length ? str2 : str1
 
  let longIndex = 0
  let shortIndex = 0
  let diff = 0
  
  for (let i = 0; i < long.length; i++) {
    if (long[longIndex] !== short[shortIndex]) {
      diff++
      if (diff > 1) return false
      
      if (long.length !== short.length) {
        longIndex++  
        continue
      }
    }
    longIndex++
    shortIndex++
  }
  return diff < 2
}

oneAway('pale', 'ple')
oneAway('pales', 'pale')
oneAway('pale', 'bale')
oneAway('pale', 'bake')
```

## Reverse Words
```javascript
const message = [ 'c', 'a', 'k', 'e', ' ',
                 'p', 'o', 'u', 'n', 'd', ' ',
                 's', 't', 'e', 'a', 'l' ];

const reverseSubarray = (array, start, end) => {
  while (start < end) {
    let temp = array[start]
    array[start] = array[end]
    array[end] = temp
    start++
    end--
  }
}

const reverseWords = message => {
  const reversedMessage = message.reverse()
  let start = 0
  
  for (let i = 0; i <= reversedMessage.length; i++) {
    if (message[i] === ' ' || i === message.length) {
      reverseSubarray(reversedMessage, start, i - 1)
      start = i + 1      
    }
  }
  return reversedMessage
}

reverseWords(message);

console.log(message.join(''));
```

## String Rotation
```javascript
const stringRotation = (str1, str2) => {
  if (str1.length !== str2.length) return false
  return (str1 + str1).includes(str2)
}

stringRotation('waterbottle', 'erbottlewat')
```

## Reverse Bits
```javascript
const decToBin = bin => (bin >>> 0).toString(2)

const reverseBits = bin => {
  let rev = 0
  
  while (bin) {
    rev <<= 1
    if (bin & 1) rev ^= 1 
    bin >>= 1
  }
  
  return rev
}

const t = reverseBits(0b1011)
decToBin(t)
```

## Merge Meeting Time
```javascript
const meetingTimes =   [
  { startTime: 0,  endTime: 1 },
  { startTime: 3,  endTime: 5 },
  { startTime: 4,  endTime: 8 },
  { startTime: 10, endTime: 12 },
  { startTime: 9,  endTime: 10 },
]

const mergeMeetingTimes = meetings => {
  if (!meetings) return []
  
  const sortedMeetings = meetings.sort((a, b) => a.startTime - b.startTime)
  
  return sortedMeetings
    .slice(1)
    .reduce((result, meeting) => {
    let current = result[result.length - 1]

    if (current.endTime < meeting.startTime) {
      result.push(meeting)
    } else {
      current.endTime = Math.max(current.endTime, meeting.endTime)
    }
    return result
  }, [meetings[0]])
}

mergeMeetingTimes(meetingTimes)
```

## IC - Reverse String In Place
```javascript
const revInPlace = chars => {
  let start = 0
  let end = chars.length - 1
  
  while (start < end) {
    let temp = chars[start]
    chars[start] = chars[end]
    chars[end] = temp
    
    start++
    end--
  }
}

const t = ['H', 'e', 'l', 'l', 'o']
revInPlace(t)
console.log(t)
```

## Clear Zero Through I
```javascript
const decToBin = bin => (bin >>> 0).toString(2)

const clearZeroThroughI = (bin, i) => {
  let mask = -1 << (i + 1)
  return bin & mask
}

const t = clearThroughI(0b110101101111, 7)
decToBin(t)
```

## Merge Arrays
```javascript
const myArray = [3, 4, 6, 10, 11, 15];
const alicesArray = [1, 5, 8, 12, 14, 19];

const mergeArrays = (arr1, arr2) => {
  if (!arr1.length) return arr2
  if (!arr2.length) return arr1
  
  let result = []
  let leftIndex = 0
  let rightIndex = 0
  
  while (leftIndex < arr1.length && rightIndex < arr2.length) {
    let leftElement = arr1[leftIndex]
    let rightElement = arr2[rightIndex]
    
    if (leftElement > rightElement) {
      result.push(rightElement)
      rightIndex++
    } else if (leftElement < rightElement) {
      result.push(leftElement)
      leftIndex++
    } else {
      result.push(leftElement)
      result.push(rightElement)
      leftIndex++
      rightIndex++
    }
  }
  
  if (leftIndex < arr1.length) {
    result = result.concat(arr1.slice(leftIndex))
  } else {
    result = result.concat(arr2.slice(rightIndex))
  }
  return result
}

console.log(mergeArrays(myArray, alicesArray));
// // logs [1, 3, 4, 5, 6, 8, 10, 11, 12, 14, 15, 19]
```

## Inflight Movie
```javascript
const inflightMovie1 = (flightLength, movies) => {
  let seen = new Set()
  
  for (let movie of movies) {
    let diff = flightLength - movie
    if (seen.has(diff)) return true 
    seen.add(movie)
  }
  return false
}

inflightMovie1(9, [1, 2, 3, 4, 5, 6, 7, 8, 9])

const inflightMovie2 = (flightLength, movies) => {
  let start = 0
  let end = movies.length - 1
  
  while (start < end) {
    let sum = movies[start] + movies[end]
    
    if (sum === flightLength) return true
    
    if (sum > flightLength) {
      end--
    } else {
      start++
    }
  }
  return false
}

inflightMovie2(9, [1, 2, 3, 4, 5, 6, 7, 8, 9])
```

## Top Scores
```javascript
const unsortedScores = [37, 89, 41, 65, 91, 53];
const HIGHEST_POSSIBLE_SCORE = 100;

const sortScores = (elements, upperBound) => {
  let buckets = Array(upperBound + 1).fill(null).map(() => [])

  return elements
    .reduce((result, element) => {
      result[element].push(element)
      return result
    }, buckets)
    .flat()
}

const radixSort = (elements, upperBound) => {
  let radix = 10
  let done = false
  let digits = 1
  
  while (!done) {
    done = true
    let buckets = Array(10).fill(null).map(() => [])
    
    for (let element of elements) {
      let remainingNumber = Math.floor(element / digits)
      let digit = remainingNumber % radix
      buckets[digit].push(element)
      
      if (remainingNumber > 0) done = false
    }
    
    elements = buckets.flat()
    digits *= 10
  }
  return elements
}

const elements = sortScores(unsortedScores, HIGHEST_POSSIBLE_SCORE)
console.log(elements)
```

## Sort a Linked List 0, 1, 2
```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
  }
}

const node1 = new Node(1)
const node2 = new Node(2)
const node3 = new Node(2)
const node4 = new Node(1)
const node5 = new Node(2)
const node6 = new Node(0)
const node7 = new Node(2)
const node8 = new Node(2)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7
node7.next = node8

const print = list => {
  while (list) {
    console.log(list.value)
    list = list.next
  }
}

const sort1 = list => {
  const counts = Array(3).fill(0)
  let curr = list
  
  while (curr) {
    counts[curr.value]++
    curr = curr.next
  }
  
  let i = 0
  while (list) {
    if (counts[i] == 0) i++

    list.value = i
    counts[i]--
    list = list.next
  }
}

const sort2 = list => {
  const zeroD = new Node(-1)
  const oneD = new Node(-1)
  const twoD = new Node(-1)
  let zero = zeroD
  let one = oneD
  let two = twoD

  while (list) {
    switch (list.value) {
      case (0):
        zero.next = list
        zero = zero.next
        break
      case (1):
        one.next = list
        one = one.next
        break
      case (2):
        two.next = list
        two = two.next
        break
    }
    list = list.next
  }

  zero.next = oneD.next
  one.next = twoD.next
  return zeroD.next
}

```

## Subarray with Sum
```javascript
const subArrayWithSum = (array, sum) => {
  let start = 0
  let end = 0
  let currSum = 0
  
  for (let i = 0; i < array.length; i++) {    
    while (currSum > sum) {
      currSum -= array[start]
      start++
    }
    
    if (currSum === sum) break
    
    currSum += array[i]
    end = i
  }
  
  return [start, end]
}

subArrayWithSum([1, 4, 20, 3, 10, 5], 33)
subArrayWithSum([1, 4, 0, 0, 3, 10, 5], 7)
subArrayWithSum([1, 4], 0)
```

## Max In Subarray of size k
```javascript
const maxInSubarray = (array, k) => {
  if (!array) return
  if (!k) return
  
  const result = [] 
  const deque = []
  
  for (let i = 0; i < k; i++) {
    while (deque.length && array[deque[deque.length - 1]] < array[i]) {
      deque.pop()
    }
    deque.push(i)
  }
  
  for (let j = k; j <= array.length; j++) {
    result.push(array[deque[0]])
    
    while (deque.length && j - k > deque[0]) {
      deque.shift()
    }
    
    while (deque.length && array[deque[deque.length - 1]] < array[j]) {
      deque.pop()
    }
    deque.push(j)
  }
  
  return result
}

maxInSubarray([1, 2, 3, 1, 4, 5, 2, 3, 6], 3) // 3 3 4 5 5 5 6
maxInSubarray([8, 5, 10, 7, 9, 4, 15, 12, 90, 13], 4) // 10 10 10 15 15 90 90

const maxSubarrays = (nums, k) => {
  if (!nums) return
  const deque = []
  const result = []

  for (let i = 0; i < k; i++) {
    if (nums[deque[deque.length - 1]] < nums[i] || !deque.length) {
      deque.push(i)
    }
  }

  result.push(nums[deque[deque.length - 1]])

  for (let j = k; j <= nums.length - 1; j++) {
    while (deque.length && deque[0] <= j - k) {
      deque.shift()
    }
    
    if (nums[deque[deque.length - 1]] < nums[j] || !deque.length) {
      deque.push(j)
    }
    result.push(nums[deque[deque.length - 1]])
  }

  return result
}

maxSubarrays([8, 5, 10, 7, 9, 4, 15, 12, 90, 13], 4)
```

## CCI 2.1
```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
  }
}

const node1 = new Node(1)
const node2 = new Node(1)
const node3 = new Node(2)
const node4 = new Node(3)
const node5 = new Node(1)
const node6 = new Node(0)
const node7 = new Node(2)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7

const print = list => {
  while (list) {
    console.log(list.value)
    list = list.next
  }
}

const removeDups1 = list => {
  if (!list) return list
  
  let previous = null
  const seen = new Set()
  
  while (list) {
    if (seen.has(list.value)) {
      previous.next = list.next
    } else {
      seen.add(list.value)
      previous = list 
    }
    list = list.next
  }
}

const removeDups2 = list => {
  if (!list) return list
  
  while (list) {
    let runner = list
    while (runner.next) {
      if (runner.next.value === list.value) {
        runner.next = runner.next.next  
      } else {
        runner = runner.next
      }
    }
    list = list.next
  }
}

removeDups2(node1)
print(node1)
```

## 268. Missing Number
```javascript
const missingNum1 = (array, n) => {
  const sum = array.reduce((result, element) => result + element, 0)
  const totalSum = (n * (n + 1)) / 2
  return totalSum - sum
}

missingNum1([1, 2, 3, 5], 5)
missingNum1([1, 2, 3, 4, 5, 6, 7, 8, 10], 10)

const missingNum2 = (array, n) => {
  let result = 0
  for (let num of array) result ^= num
  for (let i = 1; i <= n; i++) result ^= i
  return result
}

missingNum2([1, 2, 3, 5], 5)
missingNum2([1, 2, 3, 4, 5, 6, 7, 8, 10], 10)
```

## Rearrange Numbers
```javascript
const arrange = array => {
  let maxIndex = array.length - 1
  let minIndex = 0
  const maxElement = array[maxIndex] + 1

  for (let i = 0; i < array.length; i++) {     
    if (i % 2 == 0) { 
      array[i] += (array[maxIndex] % maxElement) * maxElement
      maxIndex--
      continue
    } 
    array[i] += (array[minIndex] % maxElement) * maxElement 
    minIndex++    
  } 
  
  for (let i = 0; i < array.length; i++) {
    array[i] = Math.floor(array[i] / maxElement)
  } 
}

const arr = [1, 2, 3, 4, 5, 6]
arrange(arr)
arr
```

## Linked List Pairwise Swap
```javascript
class Node {
  constructor(value) {
    this.value = value
    this.next = null
  }
}

const node1 = new Node(1)
const node2 = new Node(2)
const node3 = new Node(2)
const node4 = new Node(4)
const node5 = new Node(5)
const node6 = new Node(6)
const node7 = new Node(7)
const node8 = new Node(8)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7
node7.next = node8

const print = list => {
  while (list) {
    console.log(list.value)
    list = list.next
  }
}

const pairwiseSwap = list => {
  if (!list) return
  
  let current = list
  let head = current.next
  
  while (true) {
    let next = current.next
    let temp = next.next
    next.next = current
    
    if (!temp || !temp.next) {
      current.next = null
      break
    }
    
    current.next = temp.next
    current = temp
  }
  return head
}

const t = pairwiseSwap(node1)
print(t)
// 2 1 4 2 6 5 8 7
```

## Clear Bit at I
```javascript
const decToBin = bin => (bin >>> 0).toString(2)

const clear = (bin, i) => {
  let mask = ~(1 << i)
  return bin & mask
}

clear(0b10011, 1).toString(2)
```

## Kadanes Algo
```javascript
const kadanes = elements => {
  let globalMax = elements[0]
  let localMax = elements[0]
  
  for (let element of elements.slice(1)) {
    localMax = Math.max(element, localMax + element)
    globalMax = Math.max(globalMax, localMax)
  }
  return globalMax
}

kadanes([-2, -1, -3, -4])
```

## Subtract
```javascript
const subtract = (x, y) => (~x + 1) + y
subtract1(9, 5)
```

## Array Leaders
```javascript
const arrayLeaders = nums => {
  if (!nums) return []
  
  let result = []
  result.push(nums[nums.length - 1])
  
  let localMax = Math.max(nums[nums.length - 1], 0)
  
  for (let i = nums.length - 2; i >= 0; i--) {
    if (localMax <= nums[i]) {
      result.push(nums[i])
    }
    
    localMax = Math.max(nums[i], localMax) 
  }
  
  return result
}

leader([16, 17, 4, 3, 5, 2]) // 17 5 2
leader([1, 2, 3, 4, 0]) // 4 0
leader([7, 4, 5, 7, 3]) // 7 7 3
```

## Min Platforms
```javascript
const minPlatforms = (arr, dep) => { 
  arr.sort((a, b) => a - b)
  dep.sort((a, b) => a - b)

  let currPlatforms = 0
  let maxPlatforms = 0
  let arrI = 0
  let depI = 0

  while (arrI < arr.length - 1 && depI < arr.length - 1) { 
    if (arr[arrI] <= dep[depI]) { 
      currPlatforms++
      maxPlatforms = Math.max(currPlatforms, maxPlatforms)
      arrI++ 
      continue
    }
    currPlatforms-- 
    depI++ 
  } 

  return maxPlatforms
} 

minPlatforms([900, 940, 950, 1100, 1500, 1800], [910, 1200, 1120, 1130, 1900, 2000])
```

## Reverse Array In Groups
```javascript
const reverse = (elements, start, end) => {
  let left = start
  let right = Math.min(elements.length - 1, end)
  
  while (left < right) {
    let temp = elements[left]
    elements[left] = elements[right]
    elements[right] = temp
    
    left++
    right--
  }
  return elements
}

const reverseInGroups = (elements, k) => {
  for (let i = 0; i < elements.length; i += k) {
    let endIndex = i + k - 1
    reverse(elements, i, endIndex)
  }
  return elements
}

reverseInGroups([1, 2, 3, 4, 5], 3) // 3 2 1 5 4
reverseInGroups([10, 20, 30, 40, 50, 60], 2) // 20 10 40 30 60 50
```

## Bubble Sort
```javascript
const swap = (arr, first, second) => {
  let temp = arr[first]
  arr[first] = arr[second]
  arr[second] = temp
}

const bubbleSort = arr => {
  if (!arr.length) return
  
  for (let endIndex = arr.length - 1; endIndex > 0; endIndex--) {
    let noSwaps = true
    for (let currentIndex = 0; currentIndex < endIndex; currentIndex++) {
      const nextIndex = currentIndex + 1
      if (arr[currentIndex] > arr[nextIndex]) {
        swap(arr, currentIndex, nextIndex)
        noSwaps = false
      }
    }
    if (noSwaps) return
  }
}

const arr = [9, 3, 5, 4, 6, 5, 4, 2, 6, 7, 6, 4, 3, 2, 1]
bubbleSort(arr)
console.log(arr)
```

## Insertion Sort
```javascript
const swap = (arr, first, second) => {
  let temp = arr[first]
  arr[first] = arr[second]
  arr[second] = temp
}

const insertionSort = arr => {
  if (!arr.length) return
  
  for (let i = 1; i < arr.length; i++) {
    for (let j = i; j > 0; j--) {
      let previous = j - 1
      while (arr[j] < arr[previous]) {
        swap(arr, j, previous)
      }
    }
  }
}

const arr = [9, 3, 5, 4, 6, 5, 4, 2, 6, 7, 6, 4, 3, 2, 1]
insertionSort(arr)
console.log(arr)
```

## Selection Sort
```javascript
const selectionSort = (arr, sortBy) => {
  for (let i = 0; i < arr.length - 1; i++) {
    let lowestElementIndex = i
    for (let j = i + 1; j < arr.length; j++)
      if (sortBy(arr[j], arr[lowestElementIndex]))
        lowestElementIndex = j
    
    if (lowestElementIndex !== i)
      [arr[lowestElementIndex], arr[i]] = [arr[i], arr[lowestElementIndex]]
  }
}

const arr = [1, 6, 5, 3, 4, 8, 9, 4, 3, 2];
selectionSort(arr, (a, b) => a < b)
console.log(arr)
```

## Kth Largest
```javascript
const swap = (arr, i, j) => {
  let temp = arr[i]
  arr[i] = arr[j]
  arr[j] = temp
}

const random = (min, max) => {
  return Math.floor(Math.random() * (max - min - 1)) + min
}

const partition = (arr, start, end) => { 
  let pivot = end
  let i = start - 1

  for (let j = start; j < end; j++) {
    if (arr[j] <= arr[pivot]) {
      i++
      swap(arr, j, i)
    }
  }

  swap(arr, i + 1, pivot)
  return i + 1
}

const kthLargest = (arr, k) => {
  let start = 0
  let end = arr.length - 1
  const target = end - k
  
  while (start <= end) {
    const randomIndex = random(start, end)
    swap(arr, randomIndex, end)

    const pivot = partition(arr, start, end)
    if (pivot === target) return arr[pivot] 

    if (pivot > target) {
      end = pivot - 1
    } else {
      start = pivot + 1 
    }
  }
  return null
}

const arr = [7, 3, 5, 4, 2, 6, 4, 3, 2, 1]
kthLargest(arr, 3)
```

## NumOfPairs
```javascript
const numOfPairs = (xs, ys) => {
  xs.sort()
  ys.sort()

  let count = 0
  
  for (let y of ys) {
    if (y === 0) {
      let i = 0
      while (xs[i] <= 0) i++
      count += (1 * (xs.length - i))
    }
    
    if (y === 1) {
      let i = 0
      while (xs[i] <= 1) i++
      count += (1 * (xs.length - i))
    }    
  }
  
  for (let x of xs) {
    if (x === 0 || x === 1) continue
    
    let i = 0
    while (ys[i] < x) i++
    count += (1 * (ys.length - i))
  }
  return count
}

numOfPairs([2, 1, 6], [1, 5])
```

## Sort 1s and 0s
```javascript
const swap = (arr, i, j) => {
  let temp = arr[i]
  arr[i] = arr[j]
  arr[j] = temp
}

const sort = nums => {
  let pivot = 0
  for (let i = 0; i < nums.length; i++) {
    if (nums[i] === 1) {
      pivot = i
      break
    }
  }
  
  swap(nums, pivot, nums.length - 1)
  pivot = nums.length - 1
  
  let i = -1
  for (let j = i; j < pivot; j++) {
    if (nums[j] <= nums[pivot]) {
      i++
      swap(nums, i, j)
    }
  }
  swap(nums, i + 1, pivot)
  return nums
}

const t = [0, 2, 1, 2, 0]
sort(t)

```

## Equilibrium
```javascript
const equilibrium = nums => {
  let leftSum = 0
  let rightSum = 0
  let midPoint = Math.floor((nums.length) / 2)

  leftSum = nums
    .slice(0, midPoint)
    .reduce((result, element) => result + element, 0)

  rightSum = nums
    .slice(midPoint + 1)
    .reduce((result, element) => result + element, 0)

  while (midPoint > 0 && midPoint < nums.length - 1) {
    if (leftSum === rightSum) {
      return midPoint
    }

    if (leftSum < rightSum) {
      rightSum -= nums[midPoint]
      leftSum += nums[midPoint + 1]
      midPoint++
    } else {
      rightSum += nums[midPoint]
      leftSum -= nums[midPoint - 1]
      midPoint--
    }
  }
  return null
}

equilibrium([-7, 1, 5, 2, -4, 3, 0])
```

## Element with left side smaller and right side greater
```javascript
const leftRight = nums => {
  let leftMax = [null]
  let rightMin = nums[nums.length - 1]
  let firstIndex = null
  
  for (let i = 1; i < nums.length; i++) {
    leftMax[i] = Math.max(leftMax[i - 1] || Number.MIN_VALUE, nums[i - 1])
  }
  
  for (let i = nums.length - 2; i > 0; i--) {
    if (nums[i] > leftMax[i] && nums[i] < rightMin) {
      firstIndex = i
    }
    rightMin = Math.min(rightMin, nums[i])
  }
  
  return nums[firstIndex] || -1
}

leftRight([4, 2, 5, 7])
leftRight([11, 9, 12])
leftRight([4, 3, 2, 7, 8, 9])
```

## Largest Num from Array
```javascript
const largestNum = nums => {
  nums.sort((a, b) => {
    let ab = Number(`${a}` + `${b}`)
    let ba = Number(`${b}` + `${a}`)
    return ba - ab
  })
}

const t = [54, 546, 548, 60]
largestNum(t)
console.log(t)
```

## Chocolate Dist
```javascript
const dist = (packets, students) => {
  packets.sort((a, b) => a - b)
  
  let start = 0
  let end = students - 1  
  let minDiff = packets[end] - packets[start]
  
  while (end < packets.length - 1) {
    start++
    end++
    minDiff = Math.min(minDiff, packets[end] - packets[start])
  }
  return minDiff
}

dist([3, 4, 1, 9, 56, 7, 9, 12], 5)
```

## Rotate By 2
```javascript
const rotateByTwo = (str1, str2) => {
  if (str1.length !== str2.length) return false
  
  const str = str1.slice(-2) + str1 + str1.slice(0, 2)
  return str.includes(str2)
}

rotateByTwo('geeksforgeeks', 'geeksgeeksfor')
```

## isUnique
```javascript
const isUnique = str => {
  if (!str.length) return false
  const lowercased = str.toLowerCase()

  let bitVector = 0

  for (let char of lowercased) {
    const diff = char.charCodeAt(0) - 'a'.charCodeAt(0)
    let mask = 1 << diff

    if ((bitVector & mask) !== 0) return false
    bitVector |= mask
  }

  let count = 0
  while (bitVector) {
    bitVector = bitVector & (bitVector - 1)
    count++
  }

  return str.length === count
}

isUnique('Hello')
```

## Inversion of Array
```javascript
const mergeSort = arr => {
  if (arr.length < 2) return [arr, 0]
  let midPoint = Math.floor(arr.length / 2)
  let [leftResult, leftCount] = mergeSort(arr.slice(0, midPoint))
  let [rightResult, rightCount] = mergeSort(arr.slice(midPoint))  
  let [currResult, currCount] = merge(leftResult, rightResult)
  return [currResult, leftCount + rightCount + currCount]
}

const merge = (left, right) => {
  let result = []
  let leftIndex = 0
  let rightIndex = 0
  let count = 0

  while (leftIndex < left.length && rightIndex < right.length) {
    let leftElement = left[leftIndex]
    let rightElement = right[rightIndex]

    if (leftElement > rightElement) {
      count += (left.length - leftIndex)
      result.push(rightElement)
      rightIndex++
    } else if (leftElement < rightElement) {
      result.push(leftElement)
      leftIndex++
    } else {
      result.push(leftElement)
      result.push(rightElement)
      leftIndex++
      rightIndex++
    }
  }

  if (leftIndex < left.length) {
    result = result.concat(left.slice(leftIndex))
  } else {
    result = result.concat(right.slice(rightIndex))
  }
  return [result, count]
}

const [_, count] = mergeSort([2, 4, 1, 3, 5])
console.log(count)
```

## 1051. Height Checker
```javascript
// Counting Sort
var heightChecker = function(heights) {
    let counts = Array(101).fill(0)
    for (let height of heights) counts[height]++
    
    let sortedHeights = []
    for (let i = 0; i < counts.length; i++) {
        while (counts[i] > 0) {
            sortedHeights.push(i)
            counts[i]--
        }
    }
    
    let i = 0
    let j = 0
    let count = 0
    while (i < heights.length && j < sortedHeights.length) {
        if (heights[i] !== sortedHeights[j]) count++
        i++
        j++
    }
    return count
};

// N Log N Sort
var heightChecker = function(heights) {
  let sortedHeights = heights.map((num) => num).sort((a, b) => a - b)
  let count = 0
  for (let i = 0; i < heights.length; i++) {
    if (heights[i] !== sortedHeights[i]) count++
  }
  return count
};

heightChecker([10,6,6,10,10,9,8,8,3,3,8,2,1,5,1,9,5,2,7,4,7,7])

```

## 922. Sort Array By Parity II
```javascript
var sortArrayByParityII = function(A) {
    let i = 0
    for (let j = 1; j < A.length; j += 2) {
        if (A[j] % 2 === 0) {
            while (A[i] % 2 == 0) i += 2
            
            let temp = A[j]
            A[j] = A[i]
            A[i] = temp
        }
    }
    return A
};
```

## Leetcode #509
```javascript
// Dynamic Programming
var fib = function(N, memo = {}) {
  if (N < 2) return N
  if (memo[N]) return memo[N]

  let result = fib(N - 1, memo) + fib(N - 2, memo)
  memo[N] = result
  return result
};

fib(29)
```

```javascript
// Iterative
var fib = function(N) {
  if (N < 2) return N
  let result = [0, 1]
  
  for (let i = 0; i < N; i++) {
    result = [result[1], result[0] + result[1]]
  }
  
  return result[0]
};

fib(29)
```

## Leetcode #999
```javascript
var numRookCaptures = function(board) {
  let rookPos = null
  let count = 0

  for (let row = 0; row < board.length; row++) {
    for (let column = 0; column < board[0].length; column++) {
      if (board[row][column] === 'R') {
        rookPos = [row, column]
      }
    }
  }

  let left = rookPos[1] - 1
  let right = rookPos[1] + 1
  let leftFound = false
  let rightFound = false

  while (left > 0 || right < board[0].length) {
    if (leftFound && rightFound) break

    if (!leftFound) {
      switch (board[rookPos[0]][left]) {
        case ('p'):
          count++
          leftFound = true
          break 
        case ('B'): 
          leftFound = true
          break
      }
    }

    if (!rightFound) {
      switch (board[rookPos[0]][right]) {
        case ('p'):
          count++
          rightFound = true
          break 
        case ('B'): 
          rightFound = true
          break
      }
    }

    if (left > 0) left--
    if (right < board[0].length) right++
  }

  let up = rookPos[0] - 1
  let down = rookPos[0] + 1
  let upFound = false
  let downFound = false

  while (up > 0 || down < board.length) {
    if (upFound && downFound) break

    if (!upFound) {
      switch (board[up][rookPos[1]]) {
        case ('p'):
          count++
          upFound = true
          break 
        case ('B'): 
          upFound = true
          break
      }
    }

    if (!downFound) {
      switch (board[down][rookPos[1]]) {
        case ('p'):
          count++
          downFound = true
          break 
        case ('B'): 
          downFound = true
          break
      }
    }

    if (up > 0) up--
    if (down < board.length) down++
  }
  
  return count
};
```

## Binary To String
```javascript
const floatToBinary = num => {
  if (num >= 1 || num <= 0) {
    return null
  }

  let bin = ['0', '.']
  while (num > 0) {
    if (bin.length >= 32) return null

    let result = num * 2
    if (result >= 1) {
      bin.push('1')
      num = result - 1
      continue
    }
    bin.push('0')
    num = result
  }
  return bin.join('')
}

floatToBinary(0.625)
```

## Merge 2 Arrays O(1) Space
```javascript
const merge = (arr1, arr2) => {
  if (!arr1.length && !arr2.length) return [arr1, arr2]
  
  let curr = 0  
  while (curr < arr1.length) {
    if (arr1[curr] > arr2[0]) {
      let temp = arr1[curr]
      arr1[curr] = arr2[0]
      arr2[0] = temp
      
      arr2.sort((a, b) => a - b)
    }
    curr++
  }
  
  return [arr1, arr2]
}

merge([1, 5, 9, 10, 15, 20], [2, 3, 8, 13])
```

## Transpose A Matrix
```javascript
var transpose = function(A) {
  const result = Array(A[0].length).fill(null).map(() => [])
  
  for (let column = 0; column < A[0].length; column++) {
    for (let row = 0; row < A.length; row++) {
      result[column][row] = A[row][column]
    }
  }
  return result
};

transpose([[1,2,3],[4,5,6],[7,8,9]])
```

## 985. Sum of Even Numbers After Queries
```javascript
const isEven = num => (num & 1) === 0

var sumEvenAfterQueries = function(A, queries) {
  if (!A.length || !queries.length) return []
  
  let sum = A.reduce((result, num) => isEven(num) ? result + num : result, 0)
  
  return queries.reduce((result, [value, index]) => {
    if (isEven(A[index])) sum -= A[index]
    A[index] += value
    if (isEven(A[index])) sum += A[index]
    return result.concat([sum])
  }, [])
};
```

## 766. Toeplitz Matrix
```javascript
var isToeplitzMatrix = function(matrix) {

    for (var i = 0; i < matrix.length - 1; i++) {
        for (var j = 0; j < matrix[0].length - 1; j++) {
            if (matrix[i][j] != matrix[i+1][j+1]) {
                return false;
            }   
        }   
    }   

    return true;
};
```
## 766. Toeplitz Matrix Followup

```javascript
function arraysEqual(a, b) {
  if (a === b) return true;
  if (a == null || b == null) return false;
  if (a.length != b.length) return false;

  for (var i = 0; i < a.length; ++i) {
    if (a[i] !== b[i]) return false;
  }
  return true;
}

var isToeplitzMatrix = function(matrix) {
  let curr = matrix[0].slice(0, matrix[0].length - 1)
  for (let row = 1; row < matrix.length; row++) {
    if (!arraysEqual(curr, matrix[row].slice(1))) return false
    curr = matrix[row].slice(0, matrix[row].length - 1)
  }
  return true
};

const matrix = [
  [1,2,3,4],
  [5,1,2,3],
  [9,5,1,2]
]
isToeplitzMatrix(matrix)
```

## 1099. Two Sum Less Than K
```javascript
var twoSumLessThanK = function(A, K) {
    const counts = Array(1001).fill(0)
    for (let a of A) counts[a]++
    
    const sortedA = []
    for (let i = 0; i < counts.length; i++) {
        while (counts[i] > 0) {
            sortedA.push(i)
            counts[i]--
        }
    }
    
    let left = 0
    let right = sortedA.length - 1
    let max = -1
    
    while (left < right) {
        let sum = sortedA[left] + sortedA[right]
        if (sum >= K) {
            right--
        } else {
            max = Math.max(sum, max)
            left++
        }
    }
    return max
};
```

## 566. Reshape the Matrix
```javascript
var matrixReshape = function(nums, r, c) {
    if (r * c !== nums.length * nums[0].length) return nums
    
    const m = Array(r).fill(null).map(() => [])
    let i = 0
    
    for (let row = 0; row < nums.length; row++) {
        for (let col = 0; col < nums[0].length; col++) {
            if (m[i].length >= c) i++
            m[i].push(nums[row][col])
        }
    }
    return m
};
```

## 243. Shortest Word Distance
```javascript
var shortestDistance = function(words, word1, word2) {
    let i1 = -1
    let i2 = -1
    let dist = words.length
    
    for (let i = 0; i < words.length; i++) {
        if (words[i] === word1) i1 = i
        if (words[i] === word2) i2 = i
        if (i1 !== -1 && i2 !== -1) dist = Math.min(Math.abs(i1 - i2), dist)    
    }
    return dist
};
```

## Monotonic
```javascript
var isMonotonic = function(A) {
  let increasing = true
  let decreasing = true

  for (let i = 0; i < A.length - 1; i++) {
    if (A[i] > A[i + 1]) increasing = false
    if (A[i] < A[i + 1]) decreasing = false
  }

  return increasing || decreasing;
};

isMonotonic([6,5,4,4])
```

## 485. Max Consecutive Ones
```javascript
var findMaxConsecutiveOnes = function(nums) {
    let max = 0
    let curr = 0
    for (const num of nums) {
        if (num === 1) {
            curr++
            continue
        }
        max = Math.max(curr, max)
        curr = 0
    }
    max = Math.max(curr, max)
    return max
};

findMaxConsecutiveOnes([1,1,0,1,1,1])
```


## 448. Find All Numbers Disappeared in an Array
```javascript
var findDisappearedNumbers = function(nums) {
  for (let i = 0; i < nums.length; i++) {
    const index = Math.abs(nums[i]) - 1
    nums[index] = -1 * Math.abs(nums[index])
  }
  
  return nums.reduce((result, num, i) => {
    if (num > 0) result.push(i + 1)
    return result
  }, [])
};

findDisappearedNumbers([4,3,2,7,8,2,3,1])
```

## 283. Move Zeroes
```javascript
var moveZeroes = function(nums) {
    let i = 0
    for (let j = 0; j < nums.length; j++) {
        if (nums[i] !== 0) {
            i++
            continue
        }
        
        if (nums[j] !== 0) {
            let temp = nums[j]
            nums[j] = nums[i]
            nums[i] = temp
            i++
        }
    }
};
```

## 169. Majority Element
```javascript
var majorityElement = function(nums) {
  let count = 0
  let cand = 0

  for (let num of nums) {
      if (count === 0) { cand = num}
      count += (num === cand) ? 1 : -1
  }
  
  count = 0
  
  for (let num of nums) {
    if (num === cand) count++
  }
  
  return count > Math.floor(nums.length / 2) ? cand : null
};

majorityElement([2,1,2,4,7,7,7,7,7])
```

## 217. Contains Duplicate
```javascript
var containsDuplicate = function(nums) {
  if (!nums.length) return false
  const unique = new Set(nums)
  return nums.length !== unique.size
};
```

## 268. Missing Number
```javascript
var missingNumber = function(nums) {
  if (!nums.length) return null
  
  let missingNum = 0
  
  for (let num of nums) {
    missingNum ^= num
  }
  
  for (let i = 0; i <= nums.length; i++) {
    missingNum ^= i
  }
  return missingNum
};

missingNumber([3,0,1])
```

## 830. Positions of Large Groups
```javascript
var largeGroupPositions = function(S) {
    let start = 0
    let result = []
    
    for (let i = 1; i <= S.length; i++) {
        if (S[i] !== S[i - 1]) {
            if (i - start > 2) {
                result.push([start, i - 1])   
            }
            start = i
        }
    }
    return result
};
```
## 118. Pascal's Triangle
```javascript
var generate = function(numRows) {
    if (!numRows) return []
    
    let result = [[1]]
    
    for (let i = 1; i < numRows; i++) {
        let curr = []
        let prev = result[i-1]
        
        for (let j = 0; j <= prev.length; j++) {
            if (j <= 0 || j >= prev.length) {
                curr.push(1)
                continue
            }
            curr.push(prev[j] + prev[j-1])
        }
        result.push(curr)
    }
    return result
};
```

## 53. Maximum Subarray
```javascript
var maxSubArray = function(nums) {
    let globalMax = nums[0]
    let localMax = nums[0]
    
    for (let i = 1; i < nums.length; i++) {
        localMax = Math.max(nums[i], localMax + nums[i])
        globalMax = Math.max(localMax, globalMax)
    }
    return globalMax
};
```

## 1128. Number of Equivalent Domino Pairs
```javascript
var numEquivDominoPairs = function(dominoes) {
  if (!dominoes.length) return 0

  const seen = Array(100).fill(0)
  let pairCount = 0

  for (let [a, b] of dominoes) {
    let min = Math.min(a, b)
    let max = Math.max(a, b)
    let val = min * 10 + max
    
    if (seen[val] > 0) {
        pairCount += seen[val]  
        seen[val]++
        continue
    }
    
    seen[val] = 1
  }
  return pairCount
};
```

## 674. Longest Continuous Increasing Subsequence
```javascript
var findLengthOfLCIS = function(nums) {
  if (!nums.length) return 0
  
  let globalMax = 1
  let localMax = 1
  
  for (let i = 1; i < nums.length; i++) {    
    if (nums[i] > nums[i-1]) {
      localMax++
      globalMax = Math.max(localMax, globalMax)
      continue
    }
      
    localMax = 1
  }
  return globalMax
};
```

## 66. Plus One
```javascript
var plusOne = function(digits) {
    let carry = 1
    let result = []
    
    for (let i = digits.length - 1; i >= 0; i--) {
        let sum = digits[i] + carry
        carry = Math.floor(sum / 10)
        result.push(sum % 10)
    }
    
    if (carry) result.push(carry)
    
    return result.reverse()
};
```

## 532. K-diff Pairs in an Array
```javascript
var findPairs = function(nums, k) {
  if (k < 0) return 0

  const counts = {}
  for (let num of nums) {
    counts[num] = 1 + (counts[num] || 0)
  }  

  let pairs = 0
  for (let [num, count] of Object.entries(counts)) {
    if (k === 0) {
      if (counts[num] > 1) pairs++
      continue
    }

    if (counts[+num + k]) pairs++
  }

  return pairs
};
```

## 747. Largest Number At Least Twice of Others
```javascript
var dominantIndex = function(nums) {
  let max = 0
  let maxIndex = 0
  let secondMax = 0
  
  for (let i = 0; i < nums.length; i++) {
    if (nums[i] > max) {
      secondMax = max
      max = nums[i]
      maxIndex = i
      continue
    }
    
    if (nums[i] > secondMax) {
      secondMax = nums[i]
    }  
  }
  
  return max >= secondMax * 2 ? maxIndex : -1
};
```

## 26. Remove Duplicates from Sorted Array
```javascript
var removeDuplicates = function(nums) {
    if (!nums.length) return 0
    
    let i = 0
    for (let j = 1; j < nums.length; j++) {
        if (nums[j] !== nums[i]) nums[++i] = nums[j]
    }
    
    return i + 1
};

removeDuplicates([0,0,1,1,1,2,2,3,3,4])
```

## 724. Find Pivot Index
```javascript
var pivotIndex = function(nums) {
    let sum = nums.reduce((result, num) => result + num, 0)
    let leftSum = 0
    for (let i = 0; i < nums.length; i++) {
        if (leftSum === sum - leftSum - nums[i]) return i
        leftSum += nums[i]
    }
    return -1
};
```

## CCI: 1.2
```javascript
const counts = str => {
  return str.split('').reduce((result, char) => {
    result[char] = 1 + (result[char] || 0)
    return result
  }, {})
}

const isPermutation2 = (str1, str2) => {
  const s1Counts = counts(str1)

  for (let char of str2) {
    if (!s1Counts[char] || s1Counts[char] <= 0) return false
    s1Counts[char]--
  }
  
  return true
}
```

## 643. Maximum Average Subarray I
```javascript
var findMaxAverage = function(nums, k) {
    let maxAvg = 0
    let currSum = 0
    
    for (let i = 0; i < k; i++) currSum += nums[i]
    maxAvg = currSum / k
    
    for (let i = k; i < nums.length; i++) {
        currSum -= nums[i - k]
        currSum += nums[i]
        maxAvg = Math.max(currSum / k, maxAvg)
    }
    return maxAvg
};
```

## 34. Find First and Last Position of Element in Sorted Array
```javascript
var searchRange = function(nums, target) {
  let first = search(nums, target)
  if (first === nums.length || nums[first] !== target) return [-1, -1]
  
  let last = search(nums, target + 1)
  if (nums[last] !== target) last--
  
  return [first, last]
};

const search = (nums, target) => {
  if (!nums.length) return null

  let left = 0
  let right = nums.length - 1

  while (left < right) {
    let midPoint = Math.floor((right - left) / 2) + left
    let midValue = nums[midPoint]

    if (midValue < target) {
      left = midPoint + 1
    } else {
      right = midPoint
    }
  }
  return left
}
```

## 1150. Check If a Number Is Majority Element in a Sorted Array
```javascript
const binarySearch = (arr, target) => {
    let left = 0
    let right = arr.length
    
    while (left < right) {
       let mid = Math.floor((right - left) / 2) + left

       if (arr[mid] < target) {
           left = mid + 1
       } else {
           right = mid
       }
    }
    
    return arr[left] === target ? left : -1
}

var isMajorityElement = function(nums, target) {
    const start = binarySearch(nums, target)
    if (start === -1) return false
    return (nums[start] === nums[start + Math.floor(nums.length / 2)])
};
```

## 1122. Relative Sort Array
```javascript
// Counting
var relativeSortArray = function(arr1, arr2) {
  const counts = Array(1001).fill(0)
  arr1.forEach(num => counts[num]++)
  
  let i = 0
  for (let num of arr2) {
    while(counts[num]-- > 0) {
      arr1[i++] = num
    }
  }
  
  for (let n = 0; n < counts.length; n++) {
    while(counts[n]-- > 0) {
      arr1[i++] = n
    }
  }
  
  return arr1
};

relativeSortArray([2,3,1,3,2,4,6,7,9,2,19], [2,1,4,3,9,6])


// Hash Map
var relativeSortArray = function(arr1, arr2) {
  const arr1Counts = arr1.reduce((result, num) => {
    result[num] = 1 + (result[num] || 0)
    return result
  }, {})
  
  let i = 0
  for (let num of arr2) {
    while (arr1Counts[num]-- > 0) arr1[i++] = num
  }
  
  for (let [key, value] of Object.entries(arr1Counts)) {
    while (arr1Counts[key]-- > 0) arr1[i++] = +key
  }
  
  return arr1
};

relativeSortArray([2,3,1,3,2,4,6,7,9,2,19], [2,1,4,3,9,6])
```

## 1002. Find Common Characters
```javascript
const charCounts = word => {
    return word.split('').reduce((result, char) => {
        result[char] = 1 + (result[char] || 0)
        return result
    }, {})
}

const merge = (map1, map2) => {
    let result = {}
    
    for (let [key, value] of Object.entries(map1)) {
        if (map1[key] && map2[key]) {
            result[key] = Math.min(map1[key], map2[key])
        }
    }
    return result
}

var commonChars = function(A) {
    if (!A.length) return []
    
    let counts = charCounts(A[0])
    for (let i = 1; i < A.length; i++) {
        counts = merge(charCounts(A[i]), counts)
    }
    
    return Object.entries(counts).reduce((result, [key, value]) => {
        return result.concat(Array(value).fill(key))
    }, [])
};
```

## 1170. Compare Strings by Frequency of the Smallest Character
```javascript
const smallestChar = str => {
    if (!str.length) return null
    
    let smallest = str[0]
    for (let i = 1; i < str.length; i++) {
        smallest = smallest < str[i] ? smallest : str[i]
    }
    return smallest
}

const smallestFreq = str => {
    let smallest = smallestChar(str)
    return str.split('').reduce((result, char) => {
        return char === smallest ? result + 1 : result
    }, 0)
} 

var numSmallerByFrequency = function(queries, words) {
    queries = queries.map(query => smallestFreq(query))
    words = words.map(word => smallestFreq(word))
    
    let counts = []
    
    for (let query of queries) {
        let count = 0
        for (let word of words) {
            if (query < word) count++
        }
        counts.push(count)
    }
    return counts
};

numSmallerByFrequency(["bba","abaaaaaa","aaaaaa","bbabbabaab","aba","aa","baab","bbbbbb","aab","bbabbaabb"], ["aaabbb","aab","babbab","babbbb","b","bbbbbbbbab","a","bbbbbbbbbb","baaabbaab","aa"])
```

## 27. Remove Element
```javascript
var removeElement = function(nums, val) {
    let i = 0
    for (let j = 0; j < nums.length; j++) {
        if (nums[i] !== val) {
            i++
            continue
        }
        
        if (nums[j] !== val) {
            let temp = nums[i]
            nums[i] = nums[j]
            nums[j] = temp
            
            i++
        }
    }
    return i
};
```

## 35. Search Insert Position
```javascript
var searchInsert = function(nums, target) {
    let left = 0
    let right = nums.length
    
    while (left < right) {
        let mid = Math.floor((right - left) / 2) + left
        
        if (nums[mid] < target) {
            left = mid + 1
        } else {
            right = mid
        }
    }
    return left
};
```

## 121. Best Time to Buy and Sell Stock
```javascript
var maxProfit = function(prices) {
    if (!prices.length) return 0
    
    let maxProfit = 0
    let minPrice = prices[0]
    
    for (let i = 1; i < prices.length; i++) {
        minPrice = Math.min(minPrice, prices[i])
        maxProfit = Math.max(maxProfit, prices[i] - minPrice)
    }
    return maxProfit
};

var maxProfit = function(prices) {
    if (!prices.length) return 0
    
    let maxGlobal = 0
    let maxLocal = 0
    
    for (let i = 1; i < prices.length; i++) {
        maxLocal = Math.max(0, maxLocal + prices[i] - prices[i - 1])
        maxGlobal = Math.max(maxLocal, maxGlobal)
    }
    return maxGlobal
};
```

## 122. Best Time to Buy and Sell Stock II
```javascript
var maxProfit = function(prices) {
    let profit = 0
    
    for (let i = 1; i < prices.length; i++) {
        if (prices[i] - prices[i - 1] > 0) {
            profit += prices[i] - prices[i - 1]
        }
    }
    return profit
};
```

## 876. Middle of the Linked List
```javascript
var middleNode = function(head) {
    let fast = head
    let slow = head
    
    while (fast && fast.next) {
        fast = fast.next.next
        slow = slow.next
    }
    
    return slow
};
```

## 206. Reverse Linked List
```javascript
var reverseList = function(head) {
    let current = head
    let previous = null
    let next = null
    
    while (current) {
        next = current.next
        current.next = previous
        previous = current
        current = next
    }
    return previous
};
```

## 237. Delete Node in a Linked List
```javascript
var deleteNode = function(node) {
    node.val = node.next.val
    node.next = node.next.next
};
```

## 21. Merge Two Sorted Lists
```javascript
var mergeTwoLists = function(l1, l2) {
    const dummy = new ListNode(NaN)
    let current = dummy
    
    while (l1 && l2) {
        if (l1.val > l2.val) {
            current.next = l2
            l2 = l2.next
        } else {
            current.next = l1
            l1 = l1.next
        }
        current = current.next
    }
    
    current.next = l1 ? l1 : l2
    return dummy.next
};

// Recursive
var mergeTwoLists = function(l1, l2) {
    if (!l1) return l2
    if (!l2) return l1

    if (l1.val > l2.val) {
        l2.next = mergeTwoLists(l1, l2.next)
        return l2
    } else {
        l1.next = mergeTwoLists(l1.next, l2)
        return l1
    }
};
```

## 83. Remove Duplicates from Sorted List
```javascript
var deleteDuplicates = function(head) {
    if (!head || !head.next) return head
    
    let curr = head
    
    while (curr) {
        let runner = curr.next
        while (runner && runner.val == curr.val)
            runner = runner.next
        
        curr.next = runner
        curr = curr.next
    }
           
    return head
};
```

## 141. Linked List Cycle
```javascript
var hasCycle = function(head) {
    if (!head) return false
    
    const seen = new Set()
    
    while (head) {
        if (seen.has(head))
            return true
            
        seen.add(head)
        head = head.next
    }
    
    return false
};

var hasCycle = function(head) {
    let fast = head
    let slow = head
    
    while (fast && fast.next) {
        fast = fast.next.next
        slow = slow.next
        
        if (fast === slow) return true
    }
    return false
};
```

## CCI 1.4
```javascript
const palindromePemutation = str => {
  str = str.toLowerCase()
  
  let mask = 0
  for (const char of str) {
    if (char === ' ') continue
    const pos = char.charCodeAt(0) - 'a'.charCodeAt(0)
    mask ^= 1 << pos
  }
  
  mask &= (mask - 1)
  return mask === 0
}

palindromePemutation("Tact Coa")
```

## 234. Palindrome Linked List
```javascript
const reverseList = node => {
    let curr = node
    let prev = null
    let next = null
    
    while (curr) {
        next = curr.next
        curr.next = prev
        prev = curr
        curr = next
    }
    return prev
}

const copyList = node => {
    const dummy = new ListNode(NaN)
    let curr = dummy
    
    while (node) {
        curr.next = new ListNode(node.val)
        curr = curr.next
        
        node = node.next
    }
    
    return dummy.next
}

const isEqual = (l1, l2) => {
    while (l1 && l2) {
        if (l1.val !== l2.val)
            return false
        l1 = l1.next
        l2 = l2.next
    }
    return true    
}

var isPalindrome = function(head) {
    if (!head || !head.next) return true
    
    let l1 = head
    let l2 = reverseList(copyList(head))
    return isEqual(l1, l2)
};


const reverse = head => {
  let current = head
  let previous = null
  let next = null

  while (current) {
    next = current.next
    current.next = previous
    previous = current
    current = next
  }
  return previous
}

var isPalindrome = function(head) {
  if (!head || !head.next) return true
  
  let fast = head
  let slow = head
  
  while (fast && fast.next) {
    fast = fast.next.next
    slow = slow.next
  }
  
  if (fast) {
    slow = slow.next
  }
  
  slow = reverse(slow)
  fast = head
  
  while (slow != null) {
    if (fast.val != slow.val) return false
    fast = fast.next
    slow = slow.next
  }
  return true
};

const reverseList = node => {
    let curr = node
    let prev = null
    let next = null
    
    while (curr) {
        next = curr.next
        curr.next = prev
        prev = curr
        curr = next
    }
    return prev
}

const middle = node => {
    let fast = node
    let slow = node
    let prev = null
    
    while (fast && fast.next) {
        fast = fast.next.next
        prev = slow
        slow = slow.next
    }
    return prev
}

const isEqual = (l1, l2) => {
    while (l1 && l2) {
        if (l1.val !== l2.val)
            return false
        
        l1 = l1.next
        l2 = l2.next
    }

    return true
}

var isPalindrome = function(head) {
    if (!head || !head.next) return true
    
    let beforeMiddle = middle(head)
    let mid = beforeMiddle.next
    beforeMiddle.next = null
    
    const reversedList = reverseList(mid)
    return isEqual(head, reversedList)
};
```

## 203. Remove Linked List Elements
```javascript
var removeElements = function(head, val) {
    if (!head) return null
    
    const preHead = new ListNode(-1)
    let curr = preHead
    
    while (head) {
        if (head.val !== val) {
            curr.next = head
            curr = curr.next
        }
        head = head.next
    }
    curr.next = null
    
    return preHead.next
};

var removeElements = function(head, val) {
    if (!head) return head
    
    const dummy = new ListNode(NaN)
    dummy.next = head
    let curr = dummy
    
    while (curr && curr.next) {
        let next = curr.next
        while (next && next.val === val)
            next = next.next
        
        curr.next = next
        curr = curr.next
    }
    
    return dummy.next
};
```

## 160. Intersection of Two Linked Lists
```javascript
// With Length
const count = list => {
    let count = 0
    
    while (list) {
        count++
        list = list.next
    }
    
    return count
}

var getIntersectionNode = function(headA, headB) {
    if (!headA || !headB) return null
    
    let headACount = count(headA)
    let headBCount = count(headB)
    let diff = Math.abs(headACount - headBCount)
    
    let largerList = headACount > headBCount ? headA : headB
    let shorterList = headACount > headBCount ? headB : headA

    while (diff) {
        largerList = largerList.next
        diff--
    }

    while (largerList && shorterList) {
        if (largerList === shorterList) return largerList
        
        largerList = largerList.next
        shorterList = shorterList.next
    }
    return null
};

// Without Length
var getIntersectionNode = function(headA, headB) {
    if (headA == null || headB == null) return null
    
    let a = headA
    let b = headB
    
    while (a !== b) {
        a = a ? a.next : headB
        b = b ? b.next : headA
    }
    return a
};
```

## 933. Number of Recent Calls
```javascript

var RecentCounter = function() {
    this.queue = []
};

/** 
 * @param {number} t
 * @return {number}
 */
RecentCounter.prototype.ping = function(t) {
    this.queue.push(t)
    
    while (this.queue[0] < t - 3000) {
        this.queue.shift()
    }
    return this.queue.length
};

/** 
 * Your RecentCounter object will be instantiated and called as such:
 * var obj = new RecentCounter()
 * var param_1 = obj.ping(t)
 */
```

## 346. Moving Average from Data Stream
```javascript
/**
 * Initialize your data structure here.
 * @param {number} size
 */
var MovingAverage = function(size) {
    this.size = size
    this.storage = []
    this.currSum = 0
};

/** 
 * @param {number} val
 * @return {number}
 */
MovingAverage.prototype.next = function(val) {
    this.storage.push(val)
    this.currSum += val
    
    while(this.storage.length > this.size) {
        let popVal = this.storage.shift()
        this.currSum -= popVal
    }
    
    return this.currSum / this.storage.length
};

/** 
 * Your MovingAverage object will be instantiated and called as such:
 * var obj = new MovingAverage(size)
 * var param_1 = obj.next(val)
 */
```

## 1021. Remove Outermost Parentheses
```javascript
var removeOuterParentheses = function(S) {
    const result = []
    let opened = 0
    
    for (let char of S) {
        if (char == '(' && opened++ > 0) result.push(char)
        if (char == ')' && opened-- > 1) result.push(char)
    }
    
    return result.join('')
};
```

## 1047. Remove All Adjacent Duplicates In String
```javascript
var removeDuplicates = function(S) {
    if (!S.length) return S
    
    const charArr = S.split('')
    const result = []
    
    for (let char of charArr) {
        if (result[result.length - 1] === char) {
            result.pop()
            continue
        }
        result.push(char)
    } 
    return result.join('')
};
```

## 232. Implement Queue using Stacks
```javascript
/**
 * Initialize your data structure here.
 */
var MyQueue = function() {
    this.left = []
    this.right = []
};

/**
 * Push element x to the back of queue. 
 * @param {number} x
 * @return {void}
 */
MyQueue.prototype.push = function(x) {
    this.left.push(x)
};

/**
 * Removes the element from in front of queue and returns that element.
 * @return {number}
 */
MyQueue.prototype.pop = function() {
    if (!this.right.length) this.shift()
    return this.right.pop()
};

/**
 * Get the front element.
 * @return {number}
 */
MyQueue.prototype.peek = function() {
    if (!this.right.length) this.shift()
    return this.right[this.right.length - 1]
};

/**
 * Returns whether the queue is empty.
 * @return {boolean}
 */
MyQueue.prototype.empty = function() {
    return this.left.length == 0 && this.right.length == 0
};

MyQueue.prototype.shift = function() {
    while (this.left.length) {
        this.right.push(this.left.pop())
    }
}

/** 
 * Your MyQueue object will be instantiated and called as such:
 * var obj = new MyQueue()
 * obj.push(x)
 * var param_2 = obj.pop()
 * var param_3 = obj.peek()
 * var param_4 = obj.empty()
 */
```

## 225. Implement Stack using Queues
```javascript
/**
 * Initialize your data structure here.
 */
var MyStack = function() {
    this.left = []
    this.right = []
};

/**
 * Push element x onto stack. 
 * @param {number} x
 * @return {void}
 */
MyStack.prototype.push = function(x) {
    this.left.push(x)
};

/**
 * Removes the element on top of the stack and returns that element.
 * @return {number}
 */
MyStack.prototype.pop = function() {
    while (this.left.length > 1) this.right.push(this.left.shift())
    let val = this.left.pop()
    let temp = this.left
    this.left = this.right
    this.right = temp
    return val
};

/**
 * Get the top element.
 * @return {number}
 */
MyStack.prototype.top = function() {
    while (this.left.length > 1) this.right.push(this.left.shift())
    return this.left[0]
};

/**
 * Returns whether the stack is empty.
 * @return {boolean}
 */
MyStack.prototype.empty = function() {
    return !this.left.length && !this.right.length
};

/** 
 * Your MyStack object will be instantiated and called as such:
 * var obj = new MyStack()
 * obj.push(x)
 * var param_2 = obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.empty()
 */
```

## 20. Valid Parentheses
```javascript
var isValid = function(s) {
    const map = { '(': ')', '{': '}', '[': ']' }
    
    const stack = []
    for (const char of s) {
        if (map[char]) {
            stack.push(char)
        } else if (map[stack.pop()] !== char) {
            return false
        }
    }
    
    return stack.length === 0
};
```

## 682. Baseball Game
```javascript
var calPoints = function(ops) {
  let stack = []
  let sum = 0

  for (let op of ops) {
    switch(op) {
        case 'C':
            let lastVal = stack.pop()
            sum -= lastVal
            break
        case 'D':
            let val = stack[stack.length - 1] * 2
            stack.push(val)
            sum += val
            break
        case '+':
            let valSum = stack[stack.length - 1] + stack[stack.length - 2]
            stack.push(valSum)
            sum += valSum
            break
        default:
            stack.push(+op)
            sum += +op   
    }     
  }
  return sum
};
```

## 155. Min Stack
```javascript
/**
 * initialize your data structure here.
 */
var MinStack = function() {
    this.storage = []
    this.min = []
};

/** 
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function(x) {
    if (!this.min.length || this.getMin() >= x) this.min.push(x)
    this.storage.push(x)
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function() {
    let val = this.storage.pop()
    if (this.getMin() === val) this.min.pop()
    return val
};

/**
 * @return {number}
 */
MinStack.prototype.top = function() {
    return this.storage[this.storage.length - 1]
};

/**
 * @return {number}
 */
MinStack.prototype.getMin = function() {
    return this.min[this.min.length - 1]
};

/** 
 * Your MinStack object will be instantiated and called as such:
 * var obj = new MinStack()
 * obj.push(x)
 * obj.pop()
 * var param_3 = obj.top()
 * var param_4 = obj.getMin()
 */
```

## CCI 1.4
```javascript
const permutationPalindrome = str => {
  let bitVector = 0
  
  for (let char of str) {
    if (char === ' ') continue
    let i = char.charCodeAt(0) - 'a'.charCodeAt(0)
    bitVector ^= 1 << i
  }
  return bitVector === 0 || (bitVector & (bitVector - 1)) === 0
}

permutationPalindrome('atco cta')
```

## 1086. High Five
```javascript
var highFive = function(items) {
    items.sort(([idA, scoreA], [idB, scoreB]) => {
        if (idA < idB) return -1
        if (idA > idB) return 1
        return scoreB - scoreA
    })
    
    const scores = items.reduce((result, [key, value]) => {
        if (!result[key]) {
            result[key] = [value]
            return result
        } else if (result[key].length < 5) {
            result[key].push(value)
        } 
        return result
    }, {})
    
    return Object.entries(scores).reduce((result, [key, value]) => {
        let sum = value.reduce((result, score) => result + score)
        result.push([key, Math.floor(sum / value.length)])
        return result
    }, []) 
};
```

## 349. Intersection of Two Arrays
```javascript
var intersection = function(nums1, nums2) {
    let seen = new Set(nums1)
    let unique = new Set(nums2.filter(num => seen.has(num)))
    return Array.from(unique)
};
```

## 976. Largest Perimeter Triangle
```javascript
var largestPerimeter = function(A) {
    A.sort((a, b) => a - b)
    
    for (let i = A.length - 3; i >= 0; i--) {
        if (A[i] + A[i+1] > A[i+2]) return A[i] + A[i+1] + A[i+2]
    }
    return 0
};
```

## 242. Valid Anagram
```javascript
const counts = str => {
    return str.split('').reduce((result, char) => {
        result[char] = 1 + (result[char] || 0)
        return result
    }, {})
}

var isAnagram = function(s, t) {
    if (s.length !== t.length) return false
    let sCounts = counts(s)
    
    for (let char of t) {
        if (!sCounts[char]) return false
        sCounts[char]--
    }
    
    for (let value of Object.values(sCounts)) {
        if (value !== 0) return false
    }
    return true
};
```

## 252. Meeting Rooms
```javascript
var canAttendMeetings = function(intervals) {
    if (intervals.length <= 1) return true
    
    intervals.sort((a, b) => a[0] - b[0])
    
    for (let i = 1; i < intervals.length; i++)
        if (intervals[i][0] < intervals[i - 1][1]) 
            return false
    
    return true
};
```

## 350. Intersection of Two Arrays II
```javascript
// Hash Map Solution
const counts = arr => {
    return arr.reduce((result, num) => {
        result[num] = 1 + (result[num] || 0)
        return result
    }, {})
}

var intersect = function(nums1, nums2) {
    const long = nums1.length > nums2.length ? nums1 : nums2
    const short = nums1.length > nums2.length ? nums2 : nums1
    const shortCounts = counts(short)
    return long.filter(num => shortCounts[num]-- > 0)
};

// Sort Solution
var intersect = function(nums1, nums2) {
    nums1.sort((a, b) => a - b)
    nums2.sort((a, b) => a - b)
    
    let result = []
    let i = 0
    let j = 0
    
    while (i < nums1.length && j < nums2.length) {
        if (nums1[i] === nums2[j]) {
            result.push(nums1[i])
            i++
            j++
            continue
        }
        
        if (nums1[i] > nums2[j]) {
            j++
            continue
        }
        
        i++
    }
    return result
};

// Binary Search
const binarySearch = (arr, target, start) => {
    let left = start || 0
    let right = arr.length
    
    while (left < right) {
        const mid = Math.floor((right - left) / 2) + left
        
        if (arr[mid] < target) {
            left = mid + 1
        } else {
            right = mid
        }
    }
    return arr[left] === target ? left : null
}

var intersect = function(nums1, nums2) {
    nums1.sort((a, b) => a - b)
    nums2.sort((a, b) => a - b)
    
    let result = []
    let start = 0
    
    for (let num of nums1) {
        let index = binarySearch(nums2, num, start)
        if (index !== null) {
            start = index + 1
            result.push(nums2[index])
        }
    }
    return result
};
```

## 344. Reverse String
```javascript
const swap = (arr, i, j) => {
    let temp = arr[i]
    arr[i] = arr[j]
    arr[j] = temp
}

var reverseString = function(s) {
    if (!s.length) return
    
    let left = 0
    let right = s.length - 1
    
    while (left < right) {
        swap(s, left, right)
        left++
        right--
    }
};

// Recursive
var reverseString = function(s) {
    const _reverseString = (s, start, end) => {
        if (start >= end) return
        [s[start], s[end]] = [s[end], s[start]]
        _reverseString(s, start + 1, end - 1)
    }
    
    _reverseString(s, 0, s.length - 1)
};
```

## 167. Two Sum II - Input array is sorted
```javascript
var twoSum = function(numbers, target) {
    if (!numbers.length) return
    
    let left = 0
    let right = numbers.length - 1
    
    while (left < right) {
        let sum = numbers[left] + numbers[right]
        
        if (sum === target) {
            return [left + 1, right + 1]
        } else if (sum > target) {
            right--
        } else {
            left++   
        }
    }
    return null
};

// Binary Search 
const binarySearch = (key, arr, start, end) => {
    while (start <= end) {
        let mid = Math.floor((end - start) / 2) + start
        if (key === arr[mid]) return mid
        
        if (key > arr[mid]) {
            start = mid + 1
        } else {
            end = mid - 1
        }
    }
    return null
}

var twoSum = function(numbers, target) {
    for (let j = 0; j < numbers.length; j++) {
        let diff = Math.abs(numbers[j] - target)
        let index = binarySearch(diff, numbers, j + 1, numbers.length)
        if (index) return [j + 1, index + 1]
    }
    return []
};
```

## 925. Long Pressed Name
```javascript
var isLongPressedName = function(name, typed) {
    let i = 0
    
    for (let j = 0; j < typed.length; j++) {
        if (typed[j] === name[i]) {
            i++
        } else if (typed[j] !== name[i - 1]){
            return false
        }
    }
    return i === name.length
};
```

## 345. Reverse Vowels of a String
```javascript
var reverseVowels = function(s) {
    if (!s.length || s.length == 1) return s
    
    const charArr = s.split('')
    const vowels = new Set(['a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'])
    
    let left = 0
    let right = s.length - 1
    
    while (left < right) {
        while (!vowels.has(charArr[left]) && left < right) left++            
        while (!vowels.has(charArr[right]) && left < right) right--
        if (left >= right) break
        
        let temp = charArr[left]
        charArr[left] = charArr[right]
        charArr[right] = temp
        left++
        right--
    }
    return charArr.join('')
};
```

## 88. Merge Sorted Array
```javascript
var merge = function(nums1, m, nums2, n) {
    let curr = nums1.length - 1
    let mI = m - 1
    let nI = n - 1
    
    while (mI >= 0 && nI >= 0) {
        nums1[curr--] = nums1[mI] < nums2[nI] ? nums2[nI--] : nums1[mI--]
    }
    
    while (nI >= 0) nums1[curr--] = nums2[nI--]
    return nums1
};
```

## 28. Implement strStr()
```javascript
// Brute Force
var strStr = function(haystack, needle) {
    if (!needle.length) return 0
    if (!haystack.length) return -1
    
    let nI = 0
    for (let hI = 0; hI < haystack.length; hI++) {
        let curr = hI
        while (haystack[curr] === needle[nI]) {
            nI++
            curr++
            if (nI >= needle.length) return hI
        }
        nI = 0
    }
    return -1
};

// KMP
const patternTable = pattern => {
  const table = [0]
  let p = 0
  let s = 1

  while (s < pattern.length) {
    if (pattern[s] === pattern[p]) {
      table[s++] = ++p
      continue
    }

    if (p === 0) {
      table[s++] = 0
      continue
    }

    p = table[p - 1]
  }
  return table
}

const kmp = (str, pattern) => {
  let table = patternTable(pattern)
  
  let p = 0
  let s = 0
  
  while (s < str.length) {
    if (str[s] === pattern[p]) {
      if (p === pattern.length - 1) return s - pattern.length + 1
      p++
      s++
      continue
    }
    
    if (p === 0) {
      s++
      continue
    }
    
    p = table[p - 1]
  }
  
  return -1
}

var strStr = function(haystack, needle) {
    if (!needle.length) return 0
    if (!haystack.length) return -1
    return kmp(haystack, needle)
};

// Rabin-Karp
var rkSearch = function (text, pattern) {
  const tLength = text.length
  const pLength = pattern.length
  
  if (tLength === 0 || pLength === 0 || tLength < pLength) 
    return null
  
  const textArr = text.split('').map(char => char.charCodeAt(0) - "0".charCodeAt(0))
  const patternArr = pattern.split('').map(char => char.charCodeAt(0) - "0".charCodeAt(0))

  const base = 256
  const prime = 101
  
  let textHash = 0
  let patternHash = 0
  
  for (let i = 0; i < pLength; i++) {
    patternHash = (base * patternHash + patternArr[i]) % prime
    textHash = (base * textHash + textArr[i]) % prime
  }
  
  const indices = []
  
  for (let i = 0; i <= tLength - pLength; i++) {
    if (textHash === patternHash) {
      if (text.substring(i, i + pLength) === pattern) {
        indices.push(i)
      }
    }
    
    let h = Math.pow(base, pLength - 1) % prime
    let nextCharCode = textArr[i + pLength]
    textHash = (base * (textHash - textArr[i] * h) + nextCharCode) % prime
    
    if (textHash < 0)
      textHash += prime 
  }
  
  return indices.length ? indices : null
}

rkSearch("aabaaabaaac", "aab")
```

## 125. Valid Palindrome
```javascript
var isPalindrome = function(s) {
    if (!s.length) return true
    s = s.toLowerCase()
    
    var regex = /\w/
    let left = 0
    let right = s.length - 1
    
    while (left < right) {
        while (!regex.test(s[left]) && left < right) left++
        while (!regex.test(s[right]) && left < right) right--
        if (s[left] !== s[right]) return false
        
        left++
        right--
    }
    return true
};
```

## 704. Binary Search
```javascript
var search = function(nums, target) {
    let start = 0
    let end = nums.length
    
    while (start <= end) {
        let mid = Math.floor((end - start) / 2) + start
        
        if (nums[mid] === target) return mid
        
        if (nums[mid] > target) {
            end = mid - 1
        } else {
            start = mid + 1
        }
    }
    return -1
};
```

## 69. Sqrt(x)
```javascript
var mySqrt = function(x) {
    if (x === 0) return 0
    
    let start = 2
    let end = Math.floor(x / 2) + 1
    
    while (start <= end) {
        let mid = Math.floor((end - start) / 2) + start
        let square = mid * mid
        if (square === x || ((mid + 1) * (mid + 1) > x && square < x)) return mid
        
        if (square > x) {
            end = mid - 1
        } else {
            start = mid + 1
        }
    }
    return start
};
```

## KMP Algo
```javascript
const substr = 'acbacdabcy'
const str = 'acbacxabcdabxabcdacbacdabcy'

const patternTable = pattern => {
  const table = [0]
  let p = 0
  let s = 1
  
  while (s < pattern.length) {
    if (pattern[p] === pattern[s]) {
      table[s] = p + 1
      s += 1
      p += 1
      continue
    }
    
    if (p === 0) {
      table[s] = 0
      s += 1
      continue
    }
    
    p = table[p - 1]
  }

  return table
}

const kmp = (str, pattern) => {
  if (pattern.length === 0) return 0
  
  const table = patternTable(pattern)
  
  let s = 0
  let p = 0
  while (s < str.length) {
    if (str[s] === pattern[p]) {
      if (p === pattern.length - 1) return (s - pattern.length) + 1
      p += 1
      s += 1
      continue
    }
    
    if (p === 0) {
      p = 0
      s += 1
      continue
    }
    
    p = table[p - 1]
  }
  return -1
}
```

## 367. Valid Perfect Square
```javascript
var isPerfectSquare = function(num) {
    if (!num) return false
    if (num === 1) return true
    
    let left = 2
    let right = Math.floor(num / 2) + 1
    
    while (left <= right) {
        let mid = Math.floor((right - left) / 2) + left
        let square = mid * mid
        
        if (square === num) return true
        
        if (square > num) {
            right = mid - 1
        } else {
            left = mid + 1
        }
    }
    return false
};
```

## 278. First Bad Version
```javascript
var solution = function(isBadVersion) {
    return function(n) {
        let left = 0
        let right = n
        
        while (left < right) {
            let mid = Math.floor((right - left) / 2) + left
            
            if (isBadVersion(mid)) {
                right = mid
            } else {
                left = mid + 1
            }
            
            
        }
        return left
    };
};
```

## 441. Arranging Coins
```javascript
var arrangeCoins = function(n) {
    let left = 0
    let right = n
    
    while (left < right) {
        let mid = Math.floor((right - left) / 2) + left
        let sum = mid * (mid + 1) / 2
        if (sum >= n) {
            right = mid
        } else {
            left = mid + 1
        }
    }
    
    let sum = (left * ((left + 1) / 2))
    return sum === n ? left : left - 1
};
```

## Rabin-Karp
```javascript
const hash = pattern => {
  const base = 256

  let hash = 0
  for (let i = 0; i < pattern.length; i += 1) {
    hash += pattern.charCodeAt(i) * (base ** i)
  }

  return hash
}

const roll = (prevHash, prevPattern, newPattern) => {
  const base = 256

  let hash = prevHash
  const prevValue = prevPattern.charCodeAt(0)
  const newValue = newPattern.charCodeAt(newPattern.length - 1)

  hash -= prevValue
  hash /= base
  hash += newValue * (base ** (newPattern.length - 1))

  return hash
}

const rabinKarp = (str, pattern) => {
  const wordHash = hash(pattern)

  let prevFrame = null
  let currentFrameHash = null

  for (let i = 0; i <= (str.length - pattern.length); i++) {
    const currentFrame = str.substring(i, i + pattern.length)

    if (currentFrameHash) {
      currentFrameHash = roll(currentFrameHash, prevFrame, currentFrame)
    } else {
      currentFrameHash = hash(currentFrame)
    }
    
    if (wordHash === currentFrameHash && currentFrame === pattern) return i
    
    prevFrame = currentFrame
  }
  
  return -1
}

rabinKarp('mississippi', 'issip')
```

## 744. Find Smallest Letter Greater Than Target
```javascript
var nextGreatestLetter = function(letters, target) {
    let left = 0
    let right = letters.length
    
    while (left < right) {
        let mid = Math.floor((right - left) / 2) + left
        
        if (letters[mid] > target) {
            right = mid
        } else {
            left = mid + 1
        }
    }
    
    return letters[left % letters.length]
};
```

## 270. Closest Binary Search Tree Value
```javascript
var closestValue = function(root, target) {
    let val = root.val
    let closest = root.val
    
    while (root) {
      val = root.val
      closest = Math.abs(val - target) < Math.abs(closest - target) ? val : closest
      root = target < root.val ? root.left : root.right
    }
    
    return closest
};
```

## 852. Peak Index in a Mountain Array
```javascript
var peakIndexInMountainArray = function(A) {
    let left = 0
    let right = A.length
    
    while (left < right) {
        let mid = Math.floor((right - left) / 2) + left
        
        if (A[mid] < A[mid + 1]) {
            left = mid + 1
        } else {
            right = mid
        }
    }
    return left
};
```

## 392. Is Subsequence
```javascript
const binarySearch = (arr, target) => {
  let left = 0
  let right = arr.length - 1
  
  while (left < right) {
    let mid = Math.floor((right - left) / 2) + left
    
    if (arr[mid] <= target) {
      left = mid + 1
    } else {
      right = mid
    }
  }
  return arr[left]
}

const indexMap = str => {
    return str.split('').reduce((result, char, i) => {
        if (result[char]) {
            result[char].push(i)
        } else {
            result[char] = [i]
        }
        return result
    }, {})
}

var isSubsequence = function(s, t) {
    const map = indexMap(t)
    
    let lastIndex = -1
    for (let i = 0; i < s.length; i++) {
        if (!map[s[i]]) return false
        
        let currIndex = binarySearch(map[s[i]], lastIndex)
        if (lastIndex >= currIndex) return false  
        lastIndex = currIndex
    }
    return true
};
```

## 1176. Diet Plan Performance
```javascript
const assignPoint = (calories, lower, upper) => {
    if (calories < lower) return -1
    if (calories > upper) return 1
    return 0
}

var dietPlanPerformance = function(calories, k, lower, upper) {
    let total = 0
    let sum = 0
    
    for (let i = 0; i < k; i++) sum += calories[i]
    total += assignPoint(sum, lower, upper)
    
    for (let j = k; j < calories.length; j++) {
        sum += calories[j]
        sum -= calories[j - k]
        total += assignPoint(sum, lower, upper)
    }
    return total
};
```

## 1064. Fixed Point
```javascript
// Linear Scan
var fixedPoint = function(A) {
    if (!A.length) return -1
    
    for (let i = 0; i < A.length; i++) {
        if (A[i] === i) return i
    }
    
    return -1
};

// Binary Search
var fixedPoint = function(A) {
    if (!A.length) return -1
    
    let left = 0
    let right = A.length
    
    while (left < right) {
        const mid = Math.floor((right - left) / 2) + left
        
        if (A[mid] < mid) {
            left = mid + 1
        } else {
            right = mid
        }
    }
    
    return left === A[left] ? left : -1
};
```

## 693. Binary Number with Alternating Bits
```javascript
var hasAlternatingBits = function(n) {
    let curr = n & 1
    n >>= 1
    
    while (n) {
        if ((n & 1) === curr) return false
        curr = n & 1
        n >>= 1
    }
    return true
};
```

## 461. Hamming Distance
```javascript
var hammingDistance = function(x, y) {
    let diff = x ^ y
    let count = 0
    while (diff) {
        count++
        diff &= (diff - 1)
    }
    return count
};
```

## 476. Number Complement
```javascript
var findComplement = function(num) {
    let numCount = 0
    let t = num
    while (t) {
        numCount++
        t >>= 1
    }
    return (num ^ ~(-1 << numCount))
};
```

## 136. Single Number
```javascript
var singleNumber = function(nums) {
    let result = 0
    for (let num of nums) result ^= num
    return result
};
```

## 762. Prime Number of Set Bits in Binary Representation
```javascript
const numOfSetBits = bin => {
  let count = 0
  while (bin) {
    if (bin & 1) count++
    bin >>= 1
  }
  return count
}

const isPrime = num => {
  if (num === 1) return false
  for (let i = 2; i <= Math.floor(num / 2); i++) {
    if (num % i === 0) return false
  }
  return true
}

var countPrimeSetBits = function(L, R) {
  let count = 0
  let memo = {}
  for (let num = L; num <= R; num++) {
      let setBits = numOfSetBits(num)
      if (memo[setBits] === undefined) memo[setBits] = isPrime(setBits)
      if (memo[setBits]) count++
  }
  return count
};
```

## 389. Find the Difference
```javascript
var findTheDifference = function(s, t) {
    let result = 0
    
    for (let i of s) result ^= i.charCodeAt(0)
    for (let j of t) result ^= j.charCodeAt(0)
    
    return String.fromCharCode(result)
};
```

## 191. Number of 1 Bits
```javascript
var hammingWeight = function(n) {
  let count = 0
  while (n) {
      count++
      n &= (n - 1)
  }
    return count
};
```

## 405. Convert a Number to Hexadecimal
```javascript
var toHex = function(num) {
    if (num === 0) return "0"
    
    let map = '0123456789abcdef'
    let result = ''
    
    while (num) {
        result = map[(num & 15)] + result
        num >>>= 4
    }
    
    return result
};
```

## 342. Power of Four
```javascript
var isPowerOfFour = function(num) {
    if (num < 1) return false
    return (num & (num - 1)) === 0 && (num & 0x55555555) === num
};
```

## 190. Reverse Bits
```javascript
var reverseBits = function(n) {
    let result = 0
    for (let i = 0; i < 32; i++) {
        result <<= 1
        result |= (n & 1)
        n >>= 1
    }
    return result >>> 0
};
```

## 1119. Remove Vowels from a String
```javascript
// RegEx
var removeVowels = function(S) {
    return S.replace(/[aeiou]/g, "")
};

// Set
var removeVowels = function(S) {
    let vowels = new Set(['a', 'e', 'i', 'o', 'u'])
    return [...S]
            .filter(char => !vowels.has(char))
            .join('')
};
```

## 1108. Defanging an IP Address
```javascript
// RegEx
var defangIPaddr = function(address) {
    return address.replace(/\./g, '[.]')
};

// Reduce
var defangIPaddr = function(address) {
    return [...address].reduce((result, char) => {
        char === '.' ? result.push('[.]') : result.push(char)
        return result
    }, []).join('')
};
```

## 804. Unique Morse Code Words
```javascript
const map = [".-","-...","-.-.","-..",".","..-.","--.","....","..",".---",
             "-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-","..-",
             "...-",".--","-..-","-.--","--.."]

const transform = word => {    
    return [...word].map(char => {
        let code = char.charCodeAt(0) - 'a'.charCodeAt(0)
        return map[code]
    }).join('')
}

var uniqueMorseRepresentations = function(words) {
    let transformations = new Set()
    for (let word of words) transformations.add(transform(word))
    return transformations.size
};
```

## 657. Robot Return to Origin
```javascript
const moveDirection = (dir, pos) => {
  let [x, y] = pos
  switch (dir) {
    case ('U'): return [x, y + 1]
    case ('D'): return [x, y - 1]
    case ('L'): return [x + 1, y]
    case ('R'): return [x - 1, y]
  }
}

var judgeCircle = function(moves) {
  let pos = [0, 0]

  for (let move of moves) pos = moveDirection(move, pos)

  let [posX, posY] = pos
  return posX === 0 && posY === 0
};
```

## 929. Unique Email Addresses
```javascript
var numUniqueEmails = function(emails) {
  const actualEmail = emails.map(email => email.split('@'))
  .map(email => [email[0].split('+')[0], email[1]])
  .map(email => [email[0].split('.').join(''), email[1]])
  .map(email => email.join('@'))

  const uniqueEmails = new Set(actualEmail)
  return uniqueEmails.size
};
```

## 557. Reverse Words in a String III
```javascript
const reverse = (arr, start, end) => {
    while (start < end) {
        let temp = arr[start]
        arr[start] = arr[end]
        arr[end] = temp
        
        start++
        end--
    }
    return arr
}

var reverseWords = function(s) {
    let charArray = [...s]
    
    let wordStart = 0
    for (let i = 0; i <= s.length; i++) {
        if (s[i] === " " || i === s.length) {
            reverse(charArray, wordStart, i - 1)
            wordStart = i + 1
        }
    }
    return charArray.join('')
};
```

## 344. Reverse String
```javascript
const swap = (arr, i, j) => {
    let temp = arr[i]
    arr[i] = arr[j]
    arr[j] = temp
}

var reverseString = function(s) {
    if (!s.length) return
    
    let left = 0
    let right = s.length - 1
    
    while (left < right) {
        swap(s, left, right)
        left++
        right--
    }
};
```

## 709. To Lower Case
```javascript
var toLowerCase = function(str) {
    let result = []
    for (let char of str) {
        let code = char.charCodeAt(0)
        if (code >= 'A'.charCodeAt(0) && code <= 'Z'.charCodeAt(0)) {
            result.push(String.fromCharCode(code + 32))
            continue
        }
        result.push(char)
    }
    return result.join('')
};
```

## 977. Squares of a Sorted Array
```javascript
var sortedSquares = function(A) {
    let right = 0
    while (A[right] < 0) right++
    
    let left = right - 1
    
    const result = []
    while (left >= 0 && right < A.length) {
        let sL = A[left] ** 2
        let sR = A[right] ** 2
        
        if (sL < sR) {
            result.push(sL)
            left--
        } else {
            result.push(sR)
            right++
        }
    }
    
    while (right < A.length) {
        result.push(A[right] ** 2)
        right++
    }
    
    while (left >= 0) {
        result.push(A[left] ** 2)
        left--
    }
    return result
};
```

## 844. Backspace String Compare
```javascript
var backspaceCompare = function(S, T) {
    let sSkips = 0
    let tSkips = 0
    
    let sIndex = S.length - 1
    let tIndex = T.length - 1
    
    while (sIndex >= 0 || tIndex >= 0) {
        if (S[sIndex] === '#') {
            sSkips++
            sIndex--
            continue
        }
        
        if (T[tIndex] === '#') {
            tSkips++
            tIndex--
            continue
        }
        
        if (sSkips > 0) {
            sSkips--
            sIndex--
            continue
        }
        
        if (tSkips > 0) {
            tSkips--
            tIndex--
            continue
        }
        
        if (S[sIndex] !== T[tIndex]) return false
        
        sIndex--
        tIndex--
    }
    return true
};
```

## 1085. Sum of Digits in the Minimum Number
```javascript
const sumDigits = num => {
    let result = 0
    while (num) {
        result += num % 10        
        num = Math.floor(num / 10)
    }
    return result
}

const isEven = num => (num & 1) === 0

var sumOfDigits = function(A) {
    let min = Math.min(...A)
    let sum = sumDigits(min)
    return isEven(sum) ? 1 : 0
};
```

## 832. Flipping an Image
```javascript
const flipRow = row => {
    for (let i = 0; i < row.length; i++) {
        row[i] = +!row[i]
    }
}

const reverseRow = row => {
    let left = 0
    let right = row.length - 1
    
    while (left < right) {
        let temp = row[left]
        row[left] = row[right]
        row[right] = temp
        
        left++
        right--
    }
}

var flipAndInvertImage = function(A) {
    if (!A.length || !A[0].length) return A
    
    for (let row of A) {
        flipRow(row)
        reverseRow(row)
    }
    return A
};
```

## 905. Sort Array By Parity
```javascript
// Linear Time and Space
const isEven = num => (num & 1) === 0

var sortArrayByParity = function(A) {
    const evens = []
    const odds = []
    for (let a of A) isEven(a) ? evens.push(a) : odds.push(a)
    return evens.concat(odds)
};

// Two Pointers
const isEven = num => (num & 1) === 0

var sortArrayByParity = function(A) {
    let i = 0
    let j = 1
    
    while (j < A.length) {
        if (isEven(A[i])) {
            i++
            j++
            continue
        }
        
        if (!isEven(A[j])) {
            j++
            continue
        }
        
        let temp = A[j]
        A[j] = A[i]
        A[i] = temp    
        
        i++
        j++
    }
    
    return A
};
```

## 561. Array Partition I
```javascript
var arrayPairSum = function(nums) {
  const arr = Array(20001).fill(0)
  const lim = 10000
  for (let num of nums) arr[num + lim]++
  
  const sortedArr = []
  for (let i = 0; i < arr.length; i++) {
    while (arr[i] > 0) {
      sortedArr.push(i - lim)
      arr[i]--
    }
  }
  
  let sum = 0
  for (let i = 0; i < sortedArr.length; i += 2) {
    sum += sortedArr[i]
  }
  return sum  
};
```

## 1160. Find Words That Can Be Formed by Characters
```javascript
const counts = str => {
    return str.split('').reduce((result, char) => {
        result[char] = 1 + (result[char] || 0)
        return result
    }, {})
}

var countCharacters = function(words, chars) {
    const charsCounts = counts(chars)
    
    let sum = 0
    for (let word of words) {
        let wordCount = counts(word)
        let valid = true
        for (let [char, count] of Object.entries(wordCount)) {
            if (!charsCounts[char] || charsCounts[char] < count) {
                valid = !valid
                break
            }   
        }
        if (valid) sum += word.length
    }
    
    return sum
};
```

## 1133. Largest Unique Number
```javascript
var largestUniqueNumber = function(A) {
    const counts = A.reduce((result, num) => {
        result[num] = 1 + (result[num] || 0)
        return result
    }, {})
    
    let max = -1
    for (let [num, count] of Object.entries(counts)) {
        if (count === 1) max = Math.max(max, num)
    }
    return max
};
```

## 509. Fibonacci Number
```javascript
// Recursion
var fib = function(N) {
    if (N < 2) return N
    return fib(N - 1) + fib(N - 2)
};

// Recursion + Memo
var fib = function(N, memo = {}) {
    if (N < 2) return N
    if (memo[N]) return memo[N]
    
    let result = fib(N - 1, memo) + fib(N - 2, memo)
    memo[N] = result
    return result
};

// Iterative
var fib = function(N) {
    let fibs = [0, 1]
    
    for (let i = 0; i < N; i++) {
        fibs = [fibs[1], fibs[0] + fibs[1]]
    }
    return fibs[0]
};
```

## 867. Transpose Matrix
```javascript
var transpose = function(A) {
    const matrix = Array(A[0].length).fill(null).map(() => [])
    for (let column = 0; column < A[0].length; column++) {
        for (let row = 0; row < A.length; row++) {
            matrix[column].push(A[row][column])
        }
    }
    return matrix
};
```

## 985. Sum of Even Numbers After Queries
```javascript
const isEven = num => (num & 1) === 0

const sumOfEvens = nums => {
    return nums.reduce((result, num) => isEven(num) ? result + num : result, 0)
}

var sumEvenAfterQueries = function(A, queries) {
    if (!A.length || !queries.length) return []
    
    const result = []
    
    let sum = sumOfEvens(A)
    for (let [val, index] of queries) {
        if (isEven(A[index])) sum -= A[index]
        A[index] += val
        if (isEven(A[index])) sum += A[index]
        result.push(sum)
    }
    return result
};
```

## 1013. Partition Array Into Three Parts With Equal Sum
```javascript
var canThreePartsEqualSum = function(A) {
    const sum = A.reduce((result, num) => result + num, 0)
    console.log(sum, sum % 3, sum / 3)
    if (sum % 3 !== 0) return false
    
    let count = 0
    let currSum = 0
    for (const a of A) {
        currSum += a
        if (currSum === sum / 3) {
            currSum = 0
            count++
        }
        
        if (count === 2) return true
    }
    return count === 3
};
```

## 896. Monotonic Array
```javascript
var isMonotonic = function(A) {
  let increasing = true
  let decreasing = true

  for (let i = 0; i < A.length; i++) {
    if (A[i] === A[i + 1]) continue
    if (A[i] > A[i + 1]) increasing = false
    if (A[i] < A[i + 1]) decreasing = false
  }

  return increasing || decreasing;
};
```

## 414. Third Maximum Number
```javascript
var thirdMax = function(nums) {
    let max1 = -Number.MAX_VALUE
    let max2 = -Number.MAX_VALUE
    let max3 = -Number.MAX_VALUE
    
    for (const num of nums) {
        if (num === max1) continue
        if (num > max1) {
            max3 = max2
            max2 = max1
            max1 = num
            continue
        }
        
        if (num === max2) continue
        if (num > max2) {
            max3 = max2
            max2 = num
            continue
        }
        
        if (num > max3) {
            max3 = num
            continue
        }
    }
    
    return max3 !== -Number.MAX_VALUE ? max3 : max1
};
```

## 119. Pascal's Triangle II
```javascript
var getRow = function(rowIndex) {
    let result = [1]
    
    for (let i = 1; i <= rowIndex; i++) {
        let row = new Array(i+1)
        for (let j = 0; j < i + 1; j++) {
            if (j <= 0 || j >= i) {
                row[j] = 1
                continue
            }
            row[j] = result[j] + result[j-1]
        }
        result = row
    }
    
    return result
};
```

## 167. Two Sum II - Input array is sorted
```javascript
var twoSum = function(numbers, target) {
    let left = 0
    let right = numbers.length - 1
    
    while (left < right) {
        let sum = numbers[left] + numbers[right]
        
        if (sum === target) return [left + 1, right + 1]
        
        if (sum > target) {
            right--
        } else {
            left++
        }
    }
    return []
};
```

## 717. 1-bit and 2-bit Characters
```javascript
var isOneBitCharacter = function(bits) {
    let i = 0
    while (i < bits.length - 1) {
        i += bits[i] + 1
    }
    return i === bits.length - 1
};
```

## 1. Two Sum
```javascript
var twoSum = function(nums, target) {
    const seen = {}
    
    for (let i = 0; i < nums.length; i++) {
        let diff = target - nums[i]
        if (seen[diff] !== undefined) return [seen[diff], i]
        seen[nums[i]] = i
    }
    return []
};
```

## 989. Add to Array-Form of Integer
```javascript
var addToArrayForm = function(A, K) {
    let carry = K
    let result = []
    
    for (let i = A.length - 1; i >= 0; i--) {
        let sum = A[i] + carry
        carry = Math.floor(sum / 10)
        result.push(sum % 10)
    }
    
    while (carry) {
        result.push(carry % 10)
        carry = Math.floor(carry / 10)
    }
    
    return result.reverse()
};
```

## CCI 2.5
```javascript
class Node {
  constructor(val) {
    this.val = val
    this.next = null
  }
}

const node1 = new Node(6)
const node2 = new Node(1)
const node3 = new Node(7)

const node4 = new Node(2)
const node5 = new Node(9)
const node6 = new Node(5)
const node7 = new Node(4)
const node8 = new Node(4)

node1.next = node2
node2.next = node3

node4.next = node5
node5.next = node6
node6.next = node7
node7.next = node8

const print = node => {
  while (node) {
    console.log(node.val)
    node = node.next
  }
}

const count = node => {
  let counter = 0
  while (node) {
    node = node.next
    counter++
  }
  return counter
}

const padZeros = (node, diff) => {
  let newHead = new Node(0)
  let curr = newHead
  while (--diff) {
    curr.next = new Node(0)
    curr = curr.next
  }
  curr.next = node
  return newHead
}

const sumLists = (list1, list2) => {
  if (!list1) return list2
  if (!list2) return list1
  
  let l1Count = count(list1)
  let l2Count = count(list2)
  
  let diff =  Math.abs(l1Count - l2Count)
  if (l1Count > l2Count) {
    list2 = padZeros(list2, diff)
  } else {
    list1 = padZeros(list1, diff)
  }
  
  let [node, carry] = _sumLists(list1, list2)
  if (carry > 0) {
    let head = new Node(carry)
    head.next = node
    return head
  }
  return node
}

const _sumLists = (list1, list2) => {
  if (!list1 && !list2) return [null, 0]

  let [node, carry] = _sumLists(list1.next, list2.next)
  let sum = list1.val + list2.val + carry
  let head = new Node(sum % 10)
  head.next = node
  return [head, Math.floor(sum / 10)]
}

print(sumLists(node1, node4))
```

## CCI 5.1
```javascript
const decToBin = bin => (bin >>> 0).toString(2)

const insertion = (n, m, i, j) => {
  let left = -1 << j + 1
  let right = (1 << i) - 1
  let mask = left | right
  n &= mask
  return n | (m << i)
}

insertion(0b100100111001, 0b1101, 1, 4)
```

## CCI 5.4
```javascript
const getNextNum = num => {
  let c = num
  let c0 = 0
  let c1 = 0
  
  while (((c & 1) === 0) && c !== 0) {
    c0++
    c >>= 1
  }
  
  while ((c & 1) === 1) {
    c1++
    c >>= 1
  }
  
  if (c0 + c1 === 31 || c0 + c1 === 0) {
    return -1
  }
  
  let p = c0 + c1
  num |= (1 << p)
  num &= ~((1 << p) - 1)
  num |= (1 << (c1 - 1)) - 1
  return num
}

getNextNum(13948)


const getPrevNum = num => {
  let temp = num
  let c0 = 0
  let c1 = 0
  
  while ((temp & 1) === 1) {
    c1++
    temp >>= 1
  }
  
  if (temp === 0)
    return -1
  
  while (((temp & 1) === 0) && temp !== 0) {
    c0++
    temp >>= 1
  }
    
  let p = c0 + c1
  num &= ~0 << p + 1
  
  let mask = (1 << (c1 + 1)) - 1
  num |= mask << (c0 - 1)
  return num
}

getPrevNum(0b10011110000011)

```

## 189. Rotate Array
```javascript
const rotateSubarray = (arr, left, right) => {
    while (left < right) {
        [arr[left], arr[right]] = [arr[right], arr[left]]
        left++
        right--
    }
}

var rotate = function(nums, k) {
    k %= nums.length
    nums.reverse()
    rotateSubarray(nums, 0, k - 1)
    rotateSubarray(nums, k, nums.length - 1)
};
```

## 605. Can Place Flowers
```javascript
var canPlaceFlowers = function(flowerbed, n) {
    let plants = 0
    
    for (let i = 0; i < flowerbed.length; i++) {        
        if (flowerbed[i] === 0 && (flowerbed[i-1] || 0) === 0 && (flowerbed[i+1] || 0) === 0) {
            plants++
            flowerbed[i] = 1
        }

        if (plants >= n) return true
    }
    
    return false
};
```

## 219. Contains Duplicate II
```javascript
var containsNearbyDuplicate = function(nums, k) {
    const set = new Set()

    for (let i = 0; i < nums.length; i++) {
        if (set.has(nums[i])) return true
        set.add(nums[i])
        if (set.size > k) set.delete(nums[i - k])
    }
    return false
};
```

## 914. X of a Kind in a Deck of Cards
```javascript
const gcd = (a, b) => {
    return a === 0 ? b : gcd(b % a, a)
}
var hasGroupsSizeX = function(deck) {
    const counts = deck.reduce((result, card) => {
        result[card] = 1 + (result[card] || 0)
        return result
    }, {})
    
    let g = 0
    for (const value of Object.values(counts)) {
       g = gcd(g, value)
    }
    return g >= 2
};
```

## 941. Valid Mountain Array
```javascript
var validMountainArray = function(A) {
    if (A.length < 3) return false
    
    let i = 0
    while (i+1 < A.length && A[i] < A[i+1]) i++
    
    if (i === 0 || i === A.length - 1) return false
    
    while (i+1 < A.length && A[i] > A[i+1]) i++    
    return i === A.length - 1
};
```

## 697. Degree of an Array
```javascript
var findShortestSubArray = function(nums) {
    let degree = 0
    let min = Number.MAX_VALUE
    
    const count = nums.reduce((result, num, i) => {
        if (!result[num]) {
            result[num] = [1, i, i]
        } else {
            result[num] = [1 + result[num][0], result[num][1], i]
        }
        degree = Math.max(degree, result[num][0])
        return result
    }, {})
    
    
    for (let value of Object.values(count)) {
        if (value[0] !== degree) continue
        min = Math.min(value[2] - value[1] + 1, min)
    }
    return min
};
```

## 532. K-diff Pairs in an Array
```javascript
var findPairs = function(nums, k) {
    if (k < 0) return 0

    const counts = nums.reduce((result, num) => {
        result[num] = 1 + (result[num] || 0)
        return result
    }, {})

    let pairs = 0
    for (let [num, count] of Object.entries(counts)) {
        if (k === 0) {
            if (counts[num] >= 2) pairs++
        } else if (counts[+num + k]) {
            pairs++
        }
    }

    return pairs
};
```

## 1185. Day of the Week
```javascript
var dayOfTheWeek = function(day, month, year) {
    var days = ['Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday']
    const date = new Date(year, month - 1, day)
    return days[date.getDay()]
};
```

## 498. Diagonal Traverse
```javascript
var findDiagonalOrder = function(matrix) {
    if (!matrix.length || !matrix[0].length) return []
    const m = matrix.length
    const n = matrix[0].length
    
    let r = 0
    let c = 0
    const result = []
    
    for (let i = 0; i < m*n; i++) {
        result.push(matrix[r][c])
        
        if ((r + c) % 2 === 0) {
            // going up
            if (c === n - 1) {
                r++
            } else if (r === 0) {
                c++
            } else {
                r--
                c++
            }
        } else {
            // going down
            if (r === m - 1) {
                c++
            } else if (c === 0) {
                r++
            } else {
                r++
                c--
            }
        }
    }
    return result
};
```

## 54. Spiral Matrix
```javascript
var spiralOrder = function(matrix) {
    if (!matrix.length || !matrix[0].length) return []
    
    const result = []
    const m = matrix.length
    const n = matrix[0].length
    let r = 0
    let c = 0
    
    for (let layer = 0; layer < Math.floor(m / 2) + 1; layer++) {
        r = layer
        c = layer
        
        if (c >= n - layer) break
        while (c < n - layer) result.push(matrix[layer][c++])
        
        r++
        c--
        if (r >= m - layer) break
        while (r < m - layer) result.push(matrix[r++][c])
        
        r--
        c--
        if (c < layer) break
        while (c >= layer) result.push(matrix[r][c--])
       
        r--
        c++
        if (r <= layer) break
        while (r > layer) result.push(matrix[r--][c])
    }
    return result
};
```

## 67. Add Binary
```javascript
var addBinary = function(a, b) {
    let i = a.length - 1
    let j = b.length - 1
    const result = []
    let carry = 0
    
    while (i >= 0 || j >= 0) {
        let iBin = a[i] ? +a[i] : 0
        let jBin = b[j] ? +b[j] : 0
        
        let sum = iBin + jBin + carry
        carry = Math.floor(sum / 2)
        result.push(sum % 2)
        
        i--
        j--
    }
    
    if (carry > 0) result.push(carry)
    return result.reverse().join('')
};
```

## 14. Longest Common Prefix
```javascript
var longestCommonPrefix = function(strs) {
    if (!strs.length) return ""
    
    let prefix = []
    let firstWord = strs[0]
    
    for (let i = 0; i < firstWord.length; i++) {
        for (let j = 1; j < strs.length; j++) {
            if (strs[j][i] !== firstWord[i]) return prefix.join('')
        }
        prefix.push(firstWord[i])
    }
    return prefix.join('')
};
```

## 209. Minimum Size Subarray Sum
```javascript
var minSubArrayLen = function(s, nums) {
    let left = 0
    let result = Number.MAX_VALUE
    let sum = 0
    
    for (let i = 0; i < nums.length; i++) {
        sum += nums[i]
        while (sum >= s) {
            result = Math.min(result, i + 1 - left)
            sum -= nums[left++]
        }
    }
    
    return result !== Number.MAX_VALUE ? result : 0
};
```

## 151. Reverse Words in a String
```javascript
const reverseSubseq = (arr, start, end) => {
    while (start < end) {
        [arr[start], arr[end]] = [arr[end], arr[start]]
        
        start++
        end--
    }
}

var reverseWords = function(s) {
    const charArr = s.split(' ')
                     .filter(str => str)
                     .reverse()
    
    let start = 0
    for (let i = 0; i < charArr.length; i++) {
        if (charArr[i] === " ") {
            reverseSubseq(charArr, start, i-1)
            start = i+1
            continue
        }
    }
    
    reverseSubseq(charArr, start, charArr.length - 1)
    
    return charArr.join(' ')
};
```

## 559. Maximum Depth of N-ary Tree
```javascript
// Iterative
var maxDepth = function(root) {
    if (!root) return 0
    
    const stack = [root]
    const seen = new Set()
    seen.add(root)
    let maxDepth = 1
    
    outer : while (stack.length) {
        let curr = stack[stack.length - 1]
        for (let child of curr.children) {
            if (!seen.has(child)) {
                stack.push(child)
                seen.add(child)
                maxDepth = Math.max(stack.length, maxDepth)
                continue outer
            }
        }
        stack.pop()
    }
    
    return maxDepth
};

// Recursive
var maxDepth = function(root) {
    if (!root) return 0
    
    let depth = 1
    
    for (const child of root.children) {
        depth = Math.max(depth, maxDepth(child) + 1)
    }
    
    return depth
};
```

## 429. N-ary Tree Level Order Traversal
```javascript
var levelOrder = function(root) {
    if (!root) return []
    
    const queue = [root]
    const result = [[root.val]]

    while (queue.length) {
        const row = []
        const size = queue.length
        for (let i = 0; i < size; i++) {
            const curr = queue.shift()
            for (const child of curr.children) {
                queue.push(child)
                row.push(child.val)
            }
        }
        if (row.length) result.push(row)
    }
    
    return result
};
```

## 111. Minimum Depth of Binary Tree
```javascript
var minDepth = function(root) {
    if (!root) return 0
    
    let leftDepth = minDepth(root.left)
    let rightDepth = minDepth(root.right)
    
    if (leftDepth === 0) return rightDepth + 1
    if (rightDepth === 0) return leftDepth + 1
    return Math.min(leftDepth + 1, rightDepth + 1)
};
```

## 107. Binary Tree Level Order Traversal II
```javascript
var levelOrderBottom = function(root) {
    if (!root) return []
    
    const queue = [root]
    const result = []
    
    while (queue.length) {
        const size = queue.length
        let row = []
        for (let i = 0; i < size; i++) {
            const curr = queue.shift()
            if (curr.left) queue.push(curr.left)
            if (curr.right) queue.push(curr.right)
            row.push(curr.val)
        }
        result.push(row)
    }
    return result.reverse()
};
```

## 101. Symmetric Tree
```javascript
// Recursive
var isSymmetric = function(root) {
    if (!root) return true
    return _isSymmetric(root.left, root.right)
};

const _isSymmetric = (left, right) => {
    if (!left && !right) return true
    if (!left || !right) return false
    return left.val === right.val &&
        _isSymmetric(left.left, right.right) && 
        _isSymmetric(left.right, right.left)
}

// Iterative
var isSymmetric = function(root) {
    if (!root) return true
    
    const queue = [root, root]
    
    while (queue.length) {
        const t1 = queue.shift()
        const t2 = queue.shift()
        
        if (!t1 && !t2) continue
        if (!t1 || !t2) return false
        if (t1.val !== t2.val) return false
        
        queue.push(t1.left)
        queue.push(t2.right)
        queue.push(t1.right)
        queue.push(t2.left)
    }
    
    return true
};
```

## 993. Cousins in Binary Tree
```javascript
var isCousins = function(root, x, y) {
    if (!root) return false
    
    const queue = [root]
    
    while (queue.length) {
        const size = queue.length
        const seen = new Set()
        
        for (let i = 0; i < size; i++) {
            const curr = queue.shift()
            seen.add(curr.val)
            if (curr.left) queue.push(curr.left)
            if (curr.right) queue.push(curr.right)
            
            let leftVal = curr.left ? curr.left.val : null
            let rightVal = curr.right ? curr.right.val : null
            if ((leftVal === x || rightVal === x) && (leftVal === y || rightVal === y)) return false
        }
        if (seen.has(x) && seen.has(y)) return true
    }
    return false
};
```

## 690. Employee Importance
```javascript
function getImportance(employees, id) {
    let map = {};
    let sum = 0;

    employees.forEach(([id, importance, reports]) => {
        map[id] = { importance, reports };
    });

    const currentEmployee = map[id];
    sum += currentEmployee.importance;

    let queue = [...currentEmployee.reports];

    while (queue.length) {
        const curId = queue.shift();
        const {importance, reports} = map[curId];
        sum += importance;
        queue.push(...reports);
    }

    return sum;
}
```

## 872. Leaf-Similar Trees
```javascript
// Iterative
var leafSimilar = function(root1, root2) {
    const leaves1 = getLeaves(root1)
    const leaves2 = getLeaves(root2)
    
    let i = 0
    let j = 0
    
    if (leaves1.length !== leaves2.length) return false
    while (i < leaves1.length && j < leaves2.length) {
        if (leaves1[i] !== leaves2[j]) return false
        i++
        j++
    }
    return true
};

const getLeaves = node => {
    const seen = new Set()
    const stack = [node]
    const leaves = []
    
    while (stack.length) {
        let curr = stack[stack.length - 1]
        
        if (!curr.left && !curr.right) {
            leaves.push(curr.val)
            stack.pop()
            continue
        }
        
        if (!seen.has(curr.left) && curr.left) {
            stack.push(curr.left)
            seen.add(curr.left)
            continue
        }
        
        if (!seen.has(curr.right) && curr.right) {
            stack.push(curr.right)
            seen.add(curr.right)
            continue
        }
        
        stack.pop()
    }
    return leaves
}

// Recursive
var leafSimilar = function(root1, root2) {
    const leaves1 = getLeaves(root1)
    const leaves2 = getLeaves(root2)

    let i = 0
    let j = 0
    
    if (leaves1.length !== leaves2.length) return false
    while (i < leaves1.length && j < leaves2.length) {
        if (leaves1[i] !== leaves2[j]) return false
        i++
        j++
    }
    return true
};

const getLeaves = node => {
    const result = []
    _getLeaves(node, result)
    return result
}

const _getLeaves = (node, result) => {
    if (!node) return
    if (!node.left && !node.right) {
        result.push(node.val)
        return
    }
    _getLeaves(node.left, result)
    _getLeaves(node.right, result)
}
```

## 112. Path Sum
```javascript
var hasPathSum = function(root, sum) {
    if (!root) return false
    
    let stack = [root]
    let seen = new Set()
    let currSum = root.val
    
    while (stack.length) {
        const curr = stack[stack.length - 1]
        
        if (!curr.left && !curr.right && currSum === sum) return true        
        
        if (!seen.has(curr.left) && curr.left) {
            seen.add(curr.left)
            stack.push(curr.left)
            currSum += curr.left.val
            continue
        }
        
        if (!seen.has(curr.right) && curr.right) {
            seen.add(curr.right)
            stack.push(curr.right)
            currSum += curr.right.val
            continue
        }
        
        currSum -= curr.val
        stack.pop()
    }
    return false
};

// Recursive
var hasPathSum = function(root, sum) {
    if (!root) return false
    if (!root.left && !root.right) return sum === root.val
    return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val)
};
```

## 257. Binary Tree Paths
```javascript
var binaryTreePaths = function(root) {
    if (!root) return []
    
    let seen = new Set()
    let stack = [root]
    
    let paths = []
    let path = [root.val]
    
    while (stack.length) {
        let curr = stack[stack.length - 1]
        
        if (!curr.left && !curr.right) {
            paths.push(path.slice())
            path.pop()
            stack.pop()
            continue
        }
        
        if (!seen.has(curr.left) && curr.left) {
            path.push(curr.left.val)
            seen.add(curr.left)
            stack.push(curr.left)
            continue
        }
        
        if (!seen.has(curr.right) && curr.right) {
            path.push(curr.right.val)
            seen.add(curr.right)
            stack.push(curr.right)
            continue
        }
        
        path.pop()
        stack.pop()
    }
    
    return paths.map(path => path.join('->'))
};
```

## 100. Same Tree
```javascript
var isSameTree = function(p, q) {
    if (!p && !q) return true
    return isEqual(preOrder(p), preOrder(q))
};

const isEqual = (a, b) => {
    if (a.length !== b.length) return false
    
    let i = 0
    let j = 0
    
    while (i < a.length && j < b.length) {
        if (a[i++] !== b[j++]) return false
    }
    return true
}

const preOrder = node => {
    const result = []
    return _preOrder(node, result)
}

const _preOrder = (node, result) => {
    if (!node) return result
    result.push(node.val)
    
    if (node.left) {
        _preOrder(node.left, result)  
    } else {
        result.push(null) 
    }
    
    if (node.right) {
      _preOrder(node.right, result)  
    } else {
        result.push(null) 
    }
    
    return result
}
```

## 144. Binary Tree Preorder Traversal
```javascript
// Iterative
var preorderTraversal = function(root) {
    let result = []
    let stack = [root]
    
    while (stack.length) {
        const curr = stack.pop()
        if (!curr) continue
        
        result.push(curr.val)
        if (curr.right) stack.push(curr.right)
        if (curr.left) stack.push(curr.left)    
    }
    return result
};

// Recursive
var preorderTraversal = function(root) {
    let result = []
    _preorderTraversal(root, result)
    return result
};

const _preorderTraversal = (node, result) => {
    if (!node) return result
    result.push(node.val)
    if (node.left) _preorderTraversal(node.left, result)
    if (node.right) _preorderTraversal(node.right, result)
}
```

## 94. Binary Tree Inorder Traversal
```javascript
// Recursive
var inorderTraversal = function(root) {
    let result = []
    _inorderTraversal(root, result)
    return result
};

const _inorderTraversal = (node, result) => {
    if (!node) return
    if (node.left) _inorderTraversal(node.left, result)
    result.push(node.val)
    if (node.right) _inorderTraversal(node.right, result)
}

// Iterative
var inorderTraversal = function(root) {
    const stack = []
    const result = []
    
    while (root || stack.length) {
        if (root) {
            stack.push(root)
            root = root.left
        } else {
            root = stack.pop()
            result.push(root.val)
            root = root.right
        }
    }
    return result
};
```

## 102. Binary Tree Level Order Traversal
```javascript
// Iterative
var levelOrder = function(root) {
    if (!root) return []
    
    const result = []
    const queue = [root]
    
    while (queue.length) {
        const size = queue.length
        const row = []
        for (let i = 0; i < size; i++) {
            const curr = queue.shift()
            
            if (curr.left) queue.push(curr.left)
            if (curr.right) queue.push(curr.right)
            
            row.push(curr.val)
        }
        result.push(row)
    }
    return result
};

// Recursive
var levelOrder = function(root) {
    if (!root) return []
    
    const result = []
    _levelOrder(root, result)
    return result
};

const _levelOrder = (node, result, level = 0) => {
    if (!node) return
    
    if (result.length === level) result.push([])
    
    result[level].push(node.val)
    
    if (node.left) _levelOrder(node.left, result, level + 1)  
    if (node.right) _levelOrder(node.right, result, level + 1)
}
```

## 145. Binary Tree Postorder Traversal
```javascript
// Iterative
var postorderTraversal = function(root) {
    let stack = []
    let result = []
    
    while (root || stack.length) {
        if (root) {
            stack.push(root)
            root = root.left
        } else {
            let temp = stack[stack.length - 1].right
            
            if (temp) {
                root = temp
            } else {
                while (stack.length && stack[stack.length - 1].right === temp) {
                    temp = stack.pop()
                    result.push(temp.val)
                }
            }
        }
    }
    
    return result
};
```

## 250. Count Univalue Subtrees
```javascript
var count = 0

var countUnivalSubtrees = function(root) {
    if (!root) return 0
    count = 0
    _countUnivalSubtrees(root)
    return count
};

const _countUnivalSubtrees = node => {
    if (!node) return null
    
    let left = _countUnivalSubtrees(node.left)
    let right = _countUnivalSubtrees(node.right)
    
    if ((left === null || node.val === left) && (right === null || node.val === right)) {
        count++
        return node.val
    }
}
```

## 116. Populating Next Right Pointers in Each Node
```javascript
var connect = function(root) {
  if (!root || !root.left) return root

  root.left.next = root.right
  root.right.next = root.next ? root.next.left : null


  connect(root.left)
  connect(root.right)
  return root
};
```

## 589. N-ary Tree Preorder Traversal
```javascript
var preorder = function(root) {
    if (!root) return []
    
    const result = []
    const stack = [root]
    
    while (stack.length) {
        const curr = stack.pop()
        result.push(curr.val)
        
        for (let i = curr.children.length - 1; i >= 0; i--)
            stack.push(curr.children[i])
    }
    
    return result
};
```

## 590. N-ary Tree Postorder Traversal
```javascript
var postorder = function(root) {
    if (!root) return []
    
    const result = []
    const stack = [root]
    
    while (stack.length) {
        const curr = stack.pop()
        result.push(curr.val)
        
        for (const child of curr.children)
            stack.push(child)
    }
    
    return result.reverse()
};
```

## 559. Maximum Depth of N-ary Tree
```javascript
var maxDepth = function(root) {
    const _maxDepth = (root, max = 1, level = 1) => {
        max = Math.max(max, level)
        
        for (const child of root.children)
            max = Math.max(_maxDepth(child, max, level + 1), max)
        
        return max
    }
    
    if (!root) return 0
    return _maxDepth(root)
};
```

## 24. Swap Nodes in Pairs
```javascript
var swapPairs = function(head) {
    if (!head || !head.next) return head

    let next = head.next.next
    let newHead = head.next
    newHead.next = head
    head.next = swapPairs(next)

    return newHead
};
```

## 25. Reverse Nodes in k-Group
```javascript
// Iterative
const reverse = (node, k) => {
    let curr = node
    let prev = null
    let next = null
    
    while (k-- && curr) {
        next = curr.next
        curr.next = prev
        prev = curr
        curr = next
    }
    node.next = curr
    return prev
}

const length = node => {
    let count = 0
    
    while (node) {
        node = node.next
        count++
    }
    return count
}

var reverseKGroup = function(head, k) {
    if (!head || k === 1) return head
    
    let count = length(head)
    let currIndex = 0
    
    let dummy = new ListNode(NaN)
    dummy.next = head
    
    let prev = dummy
    let curr = head
    
    while (curr) {
        if (count - currIndex >= k)
            prev.next = reverse(curr, k)
        
        currIndex += k
        prev = curr
        curr = curr.next
    }
    
    return dummy.next
};

// Recursive
const reverse = head => {
    let curr = head
    let prev = null
    let next = null
    
    while (curr) {
        next = curr.next
        curr.next = prev
        prev = curr
        curr = next
    }
    return prev
}

var reverseKGroup = function(head, k) {
    if (!head) return head
    
    let tail = head
    let i = 0
    while (i < k - 1 && tail) {
        tail = tail.next 
        i++
    }
    
    if (!tail || i !== k - 1) return head
    
    let nextHead = tail.next
    tail.next = null
    
    let reverseHead = reverse(head)
    head.next = reverseKGroup(nextHead, k)
    return reverseHead
};
```

## 70. Climbing Stairs
```javascript
const memo = {}

var climbStairs = function(n) {
    if (n <= 2) return n
    if (!memo[n-1]) memo[n-1] = climbStairs(n-1)
    if (!memo[n-2]) memo[n-2] = climbStairs(n-2)
    return memo[n-1] + memo[n-2]
};

// Dynamic Programming
var climbStairs = function(n) {
    const steps = [0, 1]
    
    while (n) {
        [steps[0], steps[1]] = [steps[1], steps[0] + steps[1]]
        n--
    }
    return steps[1]
};
```

## 387. First Unique Character in a String
```javascript
const counts = s => {
    return s.split('').reduce((result, element) => {
        result[element] = 1 + (result[element] || 0)
        return result
    }, {})
}

var firstUniqChar = function(s) {
    const count = counts(s)
    
    for (let i = 0; i < s.length; i++) {
        if (count[s[i]] > 1) continue
        return i
    }
    return -1
};
```

## 226. Invert Binary Tree
```javascript
var invertTree = function(root) {
    if (!root) return root
    
    const left = invertTree(root.left)
    const right = invertTree(root.right)
    
    root.left = right
    root.right = left
    
    return root
};
```

## 1165. Single-Row Keyboard
```javascript
var calculateTime = function(keyboard, word) {
    const map = keyboard.split('').reduce((result, char, i) => {
        result[char] = i
        return result
    }, {})
    
    let currIndex = 0
    let time = 0
    
    for (let char of word) {
        let index = map[char]
        let dist = Math.abs(currIndex - index)
        time += dist
        currIndex = index
    }
    
    return time
};
```

## 938. Range Sum of BST
```javascript
var rangeSumBST = function(root, L, R) {
    if (!root) return 0
    
    if (L <= root.val && root.val <= R) {
        return root.val + rangeSumBST(root.left, L, R) + rangeSumBST(root.right, L, R) 
    }
    
    if (L > root.val) {
        return rangeSumBST(root.right, L, R)
    }
    
    return rangeSumBST(root.left, L, R) 
};
```

## 1137. N-th Tribonacci Number
```javascript
const memo = {}

var tribonacci = function(n) {
    if (memo[n]) return memo[n]
    if (n <= 1) return n
    if (n === 2) return 1
    memo[n] = tribonacci(n - 3) + tribonacci(n - 2) + tribonacci(n - 1)
    return memo[n]
};
```

## 783. Minimum Distance Between BST Nodes
```javascript
var minDiffInBST = function(root) {
    let prev = null 
    let min = Number.MAX_VALUE
    
    const _minDiffInBST = root => {
        if (!root) return
        
        _minDiffInBST(root.left)
        
        if (prev) min = Math.min(min, root.val - prev)
        prev = root.val
        
        _minDiffInBST(root.right)
    }
    _minDiffInBST(root)
    return min
};

// Iterative
var minDiffInBST = function(root) {
    if (!root) return 0
    
    let stack = []
    let prev = null 
    let min = Number.MAX_VALUE
    
    
    while (root || stack.length) {
        if (root) {
            stack.push(root)
            root = root.left
        } else {
            root = stack.pop()
            
            if (prev) min = Math.min(min, root.val - prev)
            prev = root.val  
            
            root = root.right
        }
    }
    
    return min
};
```

## 617. Merge Two Binary Trees
```javascript
var mergeTrees = function(t1, t2) {
    if (!t1) return t2
    if (!t2) return t1
    
    t1.val = t1.val + t2.val
    t1.left = mergeTrees(t1.left, t2.left)
    t1.right = mergeTrees(t1.right, t2.right)
    return t1
};
```

## 700. Search in a Binary Search Tree
```javascript
var searchBST = function(root, val) {
    if (!root) return null
    
    if (root.val === val) return root
    if (root.val > val) {
        return searchBST(root.left, val)
    } else {
        return searchBST(root.right, val)
    }
};

// Iterative
var searchBST = function(root, val) {
    while (root) {
        if (root.val === val) return root
        
        if (root.val > val) {
            root = root.left
        } else {
            root = root.right
        }
    }
    return null
};
```

## 965. Univalued Binary Tree
```javascript
var isUnivalTree = function(root) {
    if (!root) return true
    if (!root.left && !root.right) return true
    
    let l = root.left ? root.left.val === root.val : true
    let r = root.right ? root.right.val === root.val : true
    if (!l || !r) return false
    
    let left = isUnivalTree(root.left)
    let right = isUnivalTree(root.right)
    return left && right
};
```

## 1022. Sum of Root To Leaf Binary Numbers
```javascript
var sumRootToLeaf = function(root) {
    const _sumRootToLeaf = (root, curr) => {
        if (!root) return
        
        curr <<= 1
        curr |= root.val
        
        if (!root.left && !root.right) {
            sum += curr
            return
        }
        
        _sumRootToLeaf(root.left, curr)    
        _sumRootToLeaf(root.right, curr)
    }
    
    let sum = 0
    _sumRootToLeaf(root, 0)
    return sum
};
```

## 637. Average of Levels in Binary Tree
```javascript
var averageOfLevels = function(root) {
    if (!root) return null
    
    const result = []
    const queue = [root]
    
    while (queue.length) {
        const size = queue.length
        let sum = 0
        for (let i = 0; i < size; i++) {
            const curr = queue.shift()
            sum += curr.val
            if (curr.left) queue.push(curr.left)
            if (curr.right) queue.push(curr.right)
        }
        result.push(sum / size)
    }
    return result
};

// Recursive
var averageOfLevels = function(root) {
    const _averageOfLevels = (root, level = 0) => {
        if (!root) return
        
        levels[level] = levels[level] ? levels[level] + root.val : root.val
        counts[level] = counts[level] ? counts[level] + 1 : 1

        _averageOfLevels(root.left, level + 1)
        _averageOfLevels(root.right, level + 1)   
    }
    
    const levels = []
    const counts = []
    _averageOfLevels(root)
    
    for (let i = 0; i < levels.length; i++)
        levels[i] = levels[i] / counts[i]
    
    return levels
};
```

## 653. Two Sum IV - Input is a BST
```javascript
var findTarget = function(root, k) {
    const inOrder = root => {
        if (!root) return
        inOrder(root.left)
        result.push(root.val)
        inOrder(root.right)
    }
        
    if (!root) return false
    
    const result = []
    inOrder(root)
    
    let left = 0
    let right = result.length - 1

    while (left < right) {
        const sum = result[left] + result[right]

        if (sum === k) return true

        if (sum > k) {
            right--
        } else {
            left++
        }
    }
        
    return false
};

// Recursive
var findTarget = function(root, k) {
    const _findTarget = (root) => {
        if (!root) return false
        
        if (set.has(k - root.val)) return true
        set.add(root.val)
        return _findTarget(root.left) || _findTarget(root.right)
    }
    
    const set = new Set()
    return _findTarget(root)
};
```

## 530. Minimum Absolute Difference in BST
```javascript
var getMinimumDifference = function(root) {
    const _getMinimumDifference = root => {
        if (!root) return
        
        _getMinimumDifference(root.left)

        if (prev !== null) diff = Math.min(root.val - prev, diff)
        prev = root.val
        
        _getMinimumDifference(root.right)
    }
    
    let prev = null
    let diff = Number.MAX_VALUE
    
    _getMinimumDifference(root)
    return diff
};

// Iterative
var getMinimumDifference = function(root) {
    const _getMinimumDifference = root => {
        if (!root) return
        _getMinimumDifference(root.left)
        result.push(root.val)
        _getMinimumDifference(root.right)
    }
    
    if (!root) return null
    
    const result = []
    _getMinimumDifference(root)
    
    let min = Number.MAX_VALUE
    for (let i = 1; i < result.length; i++) {
        min = Math.min(result[i] - result[i - 1], min)
    }
    return min
};
```

## 671. Second Minimum Node In a Binary Tree
```javascript
var findSecondMinimumValue = function(root) {
    const _findSecondMinimumValue = root => {
        if (!root) return
        
        if (min < root.val && root.val < secondMin) {
            secondMin = root.val
            return
        }
        
        if (min === root.val) {
            _findSecondMinimumValue(root.left)
            _findSecondMinimumValue(root.right)   
        }
    }
    
    if (!root) return -1
    
    const min = root.val
    let secondMin = Number.MAX_VALUE
    
    _findSecondMinimumValue(root)
    
    return secondMin === Number.MAX_VALUE ? -1 : secondMin
};
```

## 563. Binary Tree Tilt
```javascript
var findTilt = function(root) {
    const _findTilt = (root) => {
        if (!root) return 0
        
        const left = _findTilt(root.left) 
        const right = _findTilt(root.right) 
        
        sum += Math.abs(left - right)
        return left + right + root.val
    }
    
    let sum = 0
    _findTilt(root)
    return sum
};
```

## 669. Trim a Binary Search Tree
```javascript
var trimBST = function(root, L, R) {
    if (!root) return null
    if (root.val > R) return trimBST(root.left, L, R)
    if (root.val < L) return trimBST(root.right, L, R)
    
    root.left = trimBST(root.left, L, R)
    root.right = trimBST(root.right, L, R)
    
    return root
};
```

## 404. Sum of Left Leaves
```javascript
var sumOfLeftLeaves = function(root, isLeft = false) {
    if (!root) return 0
    
    if (!root.left && !root.right && isLeft) 
        return root.val
    
    return sumOfLeftLeaves(root.left, true) + sumOfLeftLeaves(root.right, false)
};
```

## 783. Minimum Distance Between BST Nodes
```javascript
var minDiffInBST = function(root) {
    const inOrder = root => {
        if (!root) return
        
        inOrder(root.left)
        
        if (prev) min = Math.min(min, root.val - prev)
        prev = root.val
        inOrder(root.right)
    }
    
    let prev = null
    let min = Number.MAX_VALUE
    inOrder(root)
    return min
};
```

## 110. Balanced Binary Tree
```javascript
var isBalanced = function(root) {
    const _isBalanced = root => {
        if (!root) return 0

        let left = _isBalanced(root.left) 
        if (left === -1) return -1

        let right = _isBalanced(root.right)
        if (right === -1) return -1

        if (Math.abs(left - right) > 1) return -1
        return Math.max(left, right) + 1
    }
    return _isBalanced(root) !== -1
};
```

## 543. Diameter of Binary Tree
```javascript
var diameterOfBinaryTree = function(root) {
    const _diameterOfBinaryTree = root => {
        if (!root) return 0
        
        const left = _diameterOfBinaryTree(root.left)
        const right = _diameterOfBinaryTree(root.right)
        
        diameter = Math.max(diameter, left + right)
        return 1 + Math.max(left, right)
    }
    
    let diameter = 0
    _diameterOfBinaryTree(root)
    return diameter
};
```

## 572. Subtree of Another Tree
```javascript
// O(mn)
var isSubtree = function(s, t) {
    const isEqual = (s, t) => {
        if (!s && !t) return true
        if (!s || !t) return false
        return s.val === t.val && 
            isEqual(s.left, t.left) && 
            isEqual(s.right, t.right)
    }
    
    const preOrder = root => {
        if (!root) return false
        if (isEqual(root, t)) return true
        
        let left = preOrder(root.left)
        let right = preOrder(root.right)
        return left || right
    }
    return preOrder(s)
};

// Contains
var isSubtree = function(s, t) {
    const buildString = (root, charArr) => {
        if (!root) {
            charArr.push('#')
            return
        }
        charArr.push(`${root.val}`)
        buildString(root.left, charArr)
        buildString(root.right, charArr)
    }
    
    const contains = (needle, haystack) => {
        let h = 0
        while (h < haystack.length) {
            let currH = h
            let n = 0
            while (n < needle.length) {
                if (needle[n] !== haystack[currH]) break
                currH++
                n++
                if (n >= needle.length) return true
            }
            h++
        }
        return false
    }
    
    const sSeralize = []
    const tSeralize = []
    
    buildString(s, sSeralize)
    buildString(t, tSeralize)
    
    return contains(tSeralize, sSeralize)
};

// KMP
var isSubtree = function(s, t) {
    const buildString = (root, charArr) => {
        if (!root) {
            charArr.push('#')
            return
        }
        
        charArr.push(`${root.val}`)
        buildString(root.left, charArr)
        buildString(root.right, charArr)
    }
    
    const kmp = (needle, haystack) => {
        const table = prefixTable(needle)
        
        let n = 0
        let h = 0
        
        while (h < haystack.length) {
            if (haystack[h] === needle[n]) {
                if (n === needle.length - 1) return true
                h++
                n++
                continue
            }
            
            if (n === 0) {
                h++
                continue
            }
            
            n = table[n - 1]
            
        }
        return false
    }
    
    const prefixTable = needle => {
        const table = [0]
        
        let p = 0
        let s = 1
        
        while (s < needle.length) {
            if (needle[s] === needle[p]) {
                table[s++] = ++p
                continue
            }
            
            if (!p) {
                table[s++] = 0
                continue
            }
            
            p = table[p - 1]
        }
        
        return table
    }
    
    const sSeralize = []
    const tSeralize = []
    
    buildString(s, sSeralize)
    buildString(t, tSeralize)
    
    return kmp(tSeralize, sSeralize)
};

// Merkle Tree
var isSubtree = function(s, t) {
    const merkle = root => {
        if (!root) return 0
        
        let left = merkle(root.left)
        let right = merkle(root.right)
        
        root.merkle = left + root.val + right
        return root.merkle
    }
    
    const dfs = root => {
        if (!root) return false
        return (root.merkle === t.merkle && isEqual(root, t)) || 
            dfs(root.left) || 
            dfs(root.right)
    }
    
    const isEqual = (s, t) => {
        if (!s && !t) return true
        if (!s || !t) return false
        return s.val === t.val && isEqual(s.left, t.left) && isEqual(s.right, t.right)
    }
    
    merkle(s)
    merkle(t)
    return dfs(s)
};
```

## 538. Convert BST to Greater Tree
```javascript
var convertBST = function(root) {
    const reverseInOrder = root => {
        if (!root) return
        reverseInOrder(root.right)
        
        let temp = root.val
        root.val = sum + root.val
        sum += temp
        
        reverseInOrder(root.left)
    }
    
    let sum = 0
    reverseInOrder(root)
    return root
};

// Morris 
var convertBST = function(root) {
    const morris = root => {
        while (root) {
            if (!root.right) {
                let temp = root.val
                root.val = sum + root.val
                sum += temp
                
                root = root.left
            } else {
                let pred = root.right
                
                while (pred.left !== root && pred.left)
                    pred = pred.left
                
                if (!pred.left) {
                    pred.left = root
                    root = root.right
                } else {
                    pred.left = null

                    let temp = root.val
                    root.val = sum + root.val
                    sum += temp
                    
                    root = root.left
                }
            }
        }
    }
    
    let sum = 0
    morris(root)
    return root
};

// Iterative
var convertBST = function(root) {
    const inOrder = root => {
        const stack = []
        
        while (stack.length || root) {
            if (!root) {
                const node = stack.pop()
                
                let temp = node.val
                node.val += sum
                sum += temp
                
                root = node.left
            } else {
                stack.push(root)
                root = root.right
            }
        }
    }
    
    let sum = 0
    inOrder(root)
    return root
};
```

## 501. Find Mode in Binary Search Tree
```javascript
var findMode = function(root) {
    const inOrder = (root, handler) => {
        while (root) {
            if (!root.left) {
                handler(root.val)
                root = root.right
            } else {
                let pred = root.left
                
                while (pred.right !== root && pred.right)
                    pred = pred.right
                
                if (!pred.right) {
                    pred.right = root
                    root = root.left
                } else {
                    handler(root.val)
                    pred.right = null
                    root = root.right
                }
            }
        }
    }
    
    const setMax = val => {
        if (val !== currVal) {
            currVal = val
            curr = 0
        }
        curr++
        max = Math.max(max, curr)
    }
    
    const addModes = val => {
        if (val !== currVal) {
            currVal = val
            curr = 0
        }
        curr++
        
        if (curr === max) result.push(currVal)
    }
    
    let currVal = null
    let curr = 0
    let max = 0
    inOrder(root, setMax)
    
    const result = []
    currVal = null
    curr = 0
    inOrder(root, addModes)
    
    return result
};
```

## 687. Longest Univalue Path
```javascript
var longestUnivaluePath = function(root) {
    const _longestUnivaluePath = (node, val) => { 
        if (!node) return 0
        
        let left = _longestUnivaluePath(node.left, node.val)
        let right = _longestUnivaluePath(node.right, node.val)
        max = Math.max(max, left + right)
        
        return node.val === val ? Math.max(left, right) + 1 : 0
    }
    
    let max = 0
    _longestUnivaluePath(root)
    return max
};
```

## 606. Construct String from Binary Tree
```javascript
var tree2str = function(t) {
    const _tree2str = (root, isRoot = true) => {
        if (!root) return
        
        if (!isRoot) s.push('(')
        s.push(root.val)
        
        if (!root.left && root.right) s.push('()')
        _tree2str(root.left, false)
        _tree2str(root.right, false)
        
        if (!isRoot) s.push(')')
    }
    
    const s = []
    _tree2str(t)
    return s.join('')
};
```

## 560. Subarray Sum Equals K
```javascript
var subarraySum = function(nums, k) {
    const map = { 0: 1 }
    let sum = 0
    let result = 0
    
    for (const num of nums) {
        sum += num
        result += (map[sum - k] || 0)
        map[sum] = 1 + (map[sum] || 0)
    }
    
    return result
};
```

## 437. Path Sum III
```javascript
var pathSum = function(root, sum) {
    const _pathSum = (root, currSum, sum, map) => {
        if (!root) return 0
        
        currSum += root.val
        paths = map[currSum - sum] || 0
        map[currSum] = 1 + (map[currSum] || 0)
        
        paths += _pathSum(root.left, currSum, sum, map)
        paths += _pathSum(root.right, currSum, sum, map)
        
        map[currSum] = map[currSum] ? map[currSum] - 1 : 0
        return paths
    }
    
    const map = { 0: 1 }
    return _pathSum(root, 0, sum, map)
};
```

## 897. Increasing Order Search Tree
```javascript
var increasingBST = function(root) {
    const _increasingBST = root => {
        if (!root) return
        _increasingBST(root.left)
        
        root.left = null
        curr.right = root
        curr = root
        
        _increasingBST(root.right)
        return root
    }
    
    const dummy = new TreeNode(0)
    let curr = dummy
    _increasingBST(root)
    return dummy.right
};
```

## 733. Flood Fill
```javascript
var floodFill = function(image, sr, sc, newColor) {
    const _floodFill = (image, cr, cc, startColor, newColor) => {
        if (cr >= image.length || cr < 0) return
        if (cc >= image[0].length || cc < 0) return
        if (image[cr][cc] !== startColor) return
        
        image[cr][cc] = newColor
        
        _floodFill(image, cr + 1, cc, startColor, newColor)
        _floodFill(image, cr - 1, cc, startColor, newColor)
        _floodFill(image, cr, cc + 1, startColor, newColor)
        _floodFill(image, cr, cc - 1, startColor, newColor)
    }
    
    if (!image || image[sr][sc] === newColor) return image
    let startColor = image[sr][sc]
    _floodFill(image, sr, sc, startColor, newColor)
    return image
};
```

## 1161. Maximum Level Sum of a Binary Tree
```javascript
var maxLevelSum = function(root) {
    const queue = [root]
    let max = 0
    let maxLevel = 0
    let level = 0
    
    while (queue.length) {
        const size = queue.length
        let sum = 0
        level++
        
        for (let i = 0; i < size; i++) {
            const curr = queue.shift()
            if (curr.left) queue.push(curr.left)
            if (curr.right) queue.push(curr.right)
            sum += curr.val
        }
        
        if (sum > max) {
            max = sum
            maxLevel = level
        }
    }
    return maxLevel
};
```

## 841. Keys and Rooms
```javascript
var canVisitAllRooms = function(rooms) {
    let stack = [0]
    let seen = new Set()
    seen.add(0)
    
    while (stack.length) {
        const curr = stack.pop()
        
        for (let key of rooms[curr]) {
            if (!seen.has(key)) {
                stack.push(key)
                seen.add(key)
                if (seen.size === rooms.length) return true
            }
        }
    }

    return seen.size === rooms.length
};
```

## 784. Letter Case Permutation
```javascript
// Iterative
var letterCasePermutation = function(S) {
    const result = [[]]
    
    for (const char of S) {
        if (Number.isNaN(+char)) {
            const n = result.length
            
            for (let i = 0; i < n; i++) {
                result.push(result[i].slice())
                result[i].push(char.toLowerCase())
                result[n + i].push(char.toUpperCase())
            }
            continue
        }
        for (const arr of result)
            arr.push(char)
    }
    
    return result.map(charArr => charArr.join(''))
};
```

## 46. Permutations
```javascript
var permute = function(nums) {
    const _permute = (nums, curr = []) => {
        if (!nums.length) {
            result.push(curr)
            return
        }
        
        for (let i = 0; i < nums.length; i++)
            _permute([...nums.slice(0, i), ...nums.slice(i + 1)], [...curr, nums[i]])
    }
    
    const result = []
    _permute(nums)
    return result
};
```

## 77. Combinations
```javascript
var combine = function(n, k) {
    const _combine = (first, curr = []) => {
        if (curr.length === k) {
            result.push(curr)
            return
        }
        
        for (let i = first; i < n + 1; i++)
            _combine(i + 1, [...curr, i])
    }
    
    const result = []
    _combine(1)
    return result
};
```

## 47. Permutations II
```javascript
var permuteUnique = function(nums) {
    const isEqual = (arr1, arr2) => {
        let i = 0
        while (i < arr1.length) {
            if (arr1[i] !== arr2[i]) return false
            i++
        }
        if (arr2.length > i) return false
        return true
    }

    const _permute = (nums, curr = []) => {
        if (!nums.length) {
            for (let arr of result)
                if (isEqual(curr, arr)) return
            
            result.push(curr)
            return
        }

        for (let i = 0; i < nums.length; i++) {
            let remaining = [...nums.slice(0, i), ...nums.slice(i + 1)]
            _permute(remaining, [...curr, nums[i]])
        }
    }

    const result = []
    _permute(nums)
    return result  
};
```

## 60. Permutation Sequence
```javascript
var getPermutation = function(n, k) {
    const _getPermutation = (nums, curr = []) => {     
        if (!nums.length) {
            result = curr
            index++
            return
        }
        
        for (let i = 0; i < nums.length; i++) {
            if (index >= k) return
            _getPermutation([...nums.slice(0, i), ...nums.slice(i + 1)], [...curr, nums[i]])
        }
    } 
    
    let index = 0
    let result = 0
    const nums = Array(n).fill(0).map((_, i) => i + 1)
    _getPermutation(nums)
    return result.join('')
};
```

## 17. Letter Combinations of a Phone Number
```javascript
var letterCombinations = function(digits) {
    const map = {
        '2': 'abc',
        '3': 'def',
        '4': 'ghi',
        '5': 'jkl',
        '6': 'mno',
        '7': 'pqrs',
        '8': 'tuv',
        '9': 'wxyz'
    }
    
    const combinations = (digits, curr = []) => {
        if (!digits.length) {
            result.push(curr.join(''))
            return
        }
        
        const letters = map[digits[0]]
        for (let i = 0; i < letters.length; i++) {
            combinations(digits.substring(1), [...curr, map[digits[0]][i]])
        }   
    }
    
    if (!digits.length) return []
    const result = []
    combinations(digits)
    return result
};
```

## 369. Plus One Linked List
```javascript
// Iterative
var plusOne = function(head) {
    const dummy = new ListNode(0)
    dummy.next = head
    
    let i = dummy
    let j = dummy
    
    while (j) {
        if (j.val < 9)
            i = j
        
        j = j.next
    }
    
    i.val++
    i = i.next
    
    while (i) {
        i.val = 0
        i = i.next
    }
    
    return dummy.val !== 0 ? dummy : dummy.next
};

// Recursive
var plusOne = function(head) {
    const _plusOne = (node) => {
        if (!node) return 1
        
        let carry = _plusOne(node.next)
        let sum = node.val + carry 
        node.val = Math.floor(sum % 10)
        carry = Math.floor(sum / 10)
        return carry
    }
    
    let remainder = _plusOne(head)
    
    if (remainder > 0) {
        const newHead = new ListNode(remainder)
        newHead.next = head
        head = newHead
    }
    return head
};
```

## 496. Next Greater Element I
```javascript
var nextGreaterElement = function(nums1, nums2) {
    const map = {}
    
    const stack = []
    for (const num of nums2) {
        while (stack[stack.length - 1] < num)
            map[stack.pop()] = num
        
        stack.push(num)
    }
    
    return nums1.map(num => map[num] === undefined ? -1 : map[num])
};
```

## 503. Next Greater Element II
```javascript
var nextGreaterElements = function(nums) {
    const n = nums.length
    const result = Array(n).fill(-1)
    const stack = []
    
    for (let i = 0; i < n*2; i++) {
        while (stack.length && nums[stack[stack.length - 1]] < nums[i % n])
            result[stack.pop()] = nums[i % n]
        
        if (i < n) stack.push(i)  
    }
    return result
};
```

## 1019. Next Greater Node In Linked List
```javascript
var nextLargerNodes = function(head) {
    const seralize = head => {
        const arr = []
        while (head) {
            arr.push(head.val)
            head = head.next
        }
        return arr
    }
    
    const arr = seralize(head)
    const result = Array(arr.length).fill(0)
    const stack = []
        
    for (let i = 0; i < arr.length; i++) {
        while (stack.length && arr[stack[stack.length - 1]] < arr[i])
            result[stack.pop()] = arr[i]
        
        stack.push(i)
    }
    
    return result
};
```

## Add Two Numbers
```javascript
var addTwoNumbers = function(l1, l2) {
    const dummy = new ListNode(-1)
    let curr = dummy
    let carry = 0
    
    while (l1 || l2) {
        let l1Val = l1 ? l1.val : 0
        let l2Val = l2 ? l2.val : 0
        let sum = carry + l1Val + l2Val
        
        let node = new ListNode(sum % 10)
        carry = Math.floor(sum / 10)
        
        curr.next = node
        curr = curr.next
        
        if (l1) l1 = l1.next
        if (l2) l2 = l2.next
    }
    
    if (carry)
        curr.next = new ListNode(carry)
    
    return dummy.next
};
```

## 445. Add Two Numbers II
```javascript
const padZeros = (node, count) => {
    let head = node
    
    while (count) {
        let zero = new ListNode(0)
        zero.next = head
        head = zero
        count--
    }
    return head
}

const count = node => {
    let count = 0
    while (node) {
        node = node.next
        count++
    }
    return count
}

var addTwoNumbers = function(l1, l2) {
    if (!l1 || !l2) return null
    
    let count1 = count(l1)
    let count2 = count(l2)
    
    let diff = Math.abs(count1 - count2)
    if (count1 < count2) {
        l1 = padZeros(l1, diff)
    } else {
        l2 = padZeros(l2, diff)
    }
    
    let [carry, nextNode] = _addTwoNumbers(l1, l2)
    
    if (carry) {
        let node = new ListNode(carry)
        node.next = nextNode
        return node
    }
    
    return nextNode
};

const _addTwoNumbers = (l1, l2) => {
    if (!l1 && !l2) return [0, null]
    
    let [carry, nextNode] = _addTwoNumbers(l1.next, l2.next)
    let sum = l1.val + l2.val + carry
    
    let currNode = new ListNode(sum % 10)
    currNode.next = nextNode
    carry = Math.floor(sum / 10)
    
    return [carry, currNode]
}
```

## Linked List Cycle II
```javascript
var detectCycle = function(head) {
    if (!head) return null
    
    let slow = head
    let fast = head
    
    while (fast && fast.next) {
        slow = slow.next
        fast = fast.next.next
        
        if (slow === fast) break
    }
    
    if (!fast || !fast.next) return null
    
    slow = head
    
    while (slow !== fast) {
        fast = fast.next
        slow = slow.next
    }
    return slow
};
```

## 430. Flatten a Multilevel Doubly Linked List
```javascript
var flatten = function(head) {
    let curr = head
    while (curr) {
        if (curr.child)
            insertAfter(curr, curr.child)
        
        curr = curr.next
    }
    return head
};

function insertAfter(head, child) {
    const next = head.next
    head.next = child
    head.child = null
    child.prev = head
    
    let tail = child 
    while (tail.next)
        tail = tail.next
    
    tail.next = next
    
    if (next)
        next.prev = tail
}
```

## Convert Sorted List to Binary Search Tree
```javascript
const seralize = node => {
    const arr = []
    while (node) {
        arr.push(node.val)
        node = node.next
    }
    return arr
}

var sortedListToBST = function(head) {
    const _sortedListToBST = arr => {
        if (!arr.length) return null
        
        const midIndex = Math.floor(arr.length / 2)
        const parent = new TreeNode(arr[midIndex])
        parent.left = _sortedListToBST(arr.slice(0, midIndex))
        parent.right = _sortedListToBST(arr.slice(midIndex + 1))
        return parent
    }
          
    const arr = seralize(head)
    return _sortedListToBST(arr)
};
```

## 328. Odd Even Linked List
```javascript
const isOdd = num => num & 1

var oddEvenList = function(head) {
    let oddDummy = new ListNode(-1)
    let evenDummy = new ListNode(-1)
    let currOdd = oddDummy
    let currEven = evenDummy
    
    let i = 1
    while (head) {
        if (isOdd(i++)) {
            currOdd.next = head
            currOdd = currOdd.next
        } else {
            currEven.next = head
            currEven = currEven.next
        }
        head = head.next
    }
    currOdd.next = null
    currEven.next = null
    
    currOdd.next = evenDummy.next
    return oddDummy.next
};
```

## 86. Partition List
```javascript
var partition = function(head, x) {
    let left = new ListNode(-1)
    let right = new ListNode(-1)
    let currL = left
    let currR = right
    
    while (head) {
        if (head.val < x) {
            currL.next = head
            currL = currL.next
        } else {
            currR.next = head
            currR = currR.next
        }
        head = head.next
    }
    currL.next = null
    currR.next = null
    
    currL.next = right.next
    return left.next
};
```

## 19. Remove Nth Node From End of List
```javascript
var removeNthFromEnd = function(head, n) {
    let dummy = new ListNode(-1)
    dummy.next = head
    
    let slow = dummy
    let fast = dummy
    
    for (let i = 0; i < n + 1; i++)
        fast = fast.next
    
    while (fast) {
        slow = slow.next
        fast = fast.next
    }
    
    slow.next = slow.next.next
    return dummy.next
};
```

## 82. Remove Duplicates from Sorted List II
```javascript
var deleteDuplicates = function(head) {
    if (!head) return head
    
    let dummy = new ListNode(-1)
    dummy.next = head
    
    let prev = dummy
    let curr = head
    let next = curr.next
    
    while (next) {
        if (curr.val !== next.val) {    
            prev = prev.next
            curr = curr.next
        } else {
            while (next && curr.val === next.val)
                next = next.next    
            
            prev.next = next
            curr = prev.next
        }
        next = curr ? curr.next : null
    }
    return dummy.next
};
```

## 426. Convert Binary Search Tree to Sorted Doubly Linked List
```javascript
// Recursive
var treeToDoublyList = function(root) {
    const _treeToDoublyList = root => {
        if (!root) return
        
        let left = root.left
        let right = root.right
        
        _treeToDoublyList(left)
        
        curr.right = root
        root.left = curr
        curr = curr.right
        
        _treeToDoublyList(right)
    }
    
    if (!root) return root
    
    const dummy = new Node(-1)
    let curr = dummy
    
    _treeToDoublyList(root)
    
    curr.right = dummy.right
    dummy.right.left = curr
    
    return dummy.right
};

// Iterative
var treeToDoublyList = function(root) {
    const _treeToDoublyList = root => {
        const stack = []
        
        while (root || stack.length) {
            if (root) {
                stack.push(root)
                root = root.left
            } else {
                const top = stack.pop()
                
                curr.right = top
                top.left = curr
                curr = curr.right
                
                root = top.right
            }
        }
    }
    
    if (!root) return root
    
    const dummy = new Node(-1)
    let curr = dummy
    
    _treeToDoublyList(root)
    
    curr.right = dummy.right
    dummy.right.left = curr
    
    return dummy.right
};
```

## 61. Rotate List
```javascript
var rotateRight = function(head, k) {
    if (!head) return head
    if (!head.next) return head
    
    let curr = head
    let n = 1
    while (curr.next) {
        curr = curr.next
        n++
    }
    curr.next = head
    
    for (let i = 0; i < n - k % n - 1; i++)
        head = head.next
    
    let newHead = head.next
    head.next = null
    return newHead
};
```

## 817. Linked List Components
```javascript
var numComponents = function(head, G) {
    const set = new Set(G)
    
    let groups = 0
    
    while (head) {    
        if (set.has(head.val) && (!head.next || !set.has(head.next.val))) 
            groups++
        
        head = head.next
    }
    return groups
};
```

## 725. Split Linked List in Parts
```javascript
const count = list => {
    let count = 0
    while (list) {
        list = list.next
        count++
    }
    return count
}

var splitListToParts = function(root, k) {
    let length = count(root)    
    let remainder = Math.floor(length % k)
    let parts = Math.floor(length / k)
    
    const result = Array(k).fill(null)
    
    for (let i = 0; i < k; i++) {
        let head = root
        for (let j = 0; j < parts + (i < remainder ? 1 : 0) - 1; j++)
            root = root.next
            
        if (root) {
            let prev = root
            root = root.next
            prev.next = null
        }
        result[i] = head
    }
    return result
};
```

## 1171. Remove Zero Sum Consecutive Nodes from Linked List
```javascript
var removeZeroSumSublists = function(head) {
    const dummy = new ListNode(-1)
    dummy.next = head
    
    const map = {0: dummy}
    
    let sum = 0
    while (head) {
        sum += head.val
        if (map[sum]) {
            let removed = map[sum].next
            let subSum = sum
            while (removed !== head) {
                subSum += removed.val
                map[subSum] = null
                removed = removed.next
            }
            map[sum].next = head.next
        } else
            map[sum] = head
        
        head = head.next
    }
    
    return dummy.next
};
```

## 138. Copy List with Random Pointer
```javascript
var copyRandomList = function(head) {
    if (!head) return head
    
    const map = new Map()
    
    let curr = head
    while (curr) {
        map.set(curr, new Node(curr.val))
        curr = curr.next
    }
    
    curr = head
    while(curr) {
        if (curr.next) 
            map.get(curr).next = map.get(curr.next)
        if (curr.random) 
            map.get(curr).random = map.get(curr.random)
        curr = curr.next
    }
    
    return map.get(head)
};

// O(1)
var copyRandomList = function(head) {
    if (!head) return head
    
    let curr = head
    while (curr) {
        let node = new Node(curr.val)
        let next = curr.next
        curr.next = node
        node.next = next
        
        curr = next
    }
    
    curr = head
    while (curr) {
        if (curr.random)
            curr.next.random = curr.random.next
        curr = curr.next.next
    }
    
    let dummy = new Node(NaN)
    let p1 = dummy
    let p2 = head
    
    while (p2) {
        p1.next = p2.next
        p1 = p1.next
        p2.next = p2.next.next
        p2 = p2.next
    }
    
    return dummy.next
};
```

## 143. Reorder List
```javascript
const reverse = node => {
    let prev = null
    let next = null
    
    while (node) {
        next = node.next
        node.next = prev
        prev = node
        node = next
    }
    return prev
}

const split = head => {
    let fast = head
    let slow = head
    let prev = null
    
    while (fast && fast.next) {
        prev = slow
        slow = slow.next
        fast = fast.next.next
    }
    
    prev.next = null
    return [head, slow]
}

const merge = (l1, l2) => {
    while (l1) {
        let nextL1 = l1.next
        let nextL2 = l2.next
        
        l1.next = l2

        if (!nextL1) break        
        l2.next = nextL1
        
        l1 = nextL1
        l2 = nextL2
    }
}

var reorderList = function(head) {
    if (!head || !head.next || !head.next.next) return head

    let [half1, half2] = split(head)
    half2 = reverse(half2)
    merge(half1, half2)
};
```

## 708. Insert into a Cyclic Sorted List
```javascript
var insert = function(head, insertVal) {
    if (!head) {
        const node = new Node(insertVal)
        node.next = node
        return node
    }
    
    let prev = head
    let next = head.next

    while (true) {
        if ((prev.val <= insertVal && insertVal <= next.val) ||
            (prev.val > next.val && insertVal < next.val) ||
            (prev.val > next.val && insertVal > prev.val)) {
            prev.next = new Node(insertVal, next)
            return head
        }
        
        prev = prev.next
        next = next.next
        if (prev === head) break
    }
    
    prev.next = new Node(insertVal, next)
    return head
};
```

## 92. Reverse Linked List II
```javascript
// Iterative
var reverseBetween = function(head, m, n) {
    if (!head) return head
    
    let dummy = new ListNode(NaN)
    dummy.next = head
    
    let prev = dummy
    
    while (--m) {
        prev = prev.next
        --n
    }
    
    let reverse = null
    let curr = prev.next
    let next = curr
    
    while (n--) {
        next = curr.next
        curr.next = reverse
        reverse = curr
        curr = next
    }
    
    prev.next.next = curr
    prev.next = reverse
    
    return dummy.next
};

// Recursion
var reverseBetween = function(head, m, n) {
    const _reverseBetween = (head, j) => {
        if (j <= 1) {
            successor = head.next
            return head
        }

        let root = _reverseBetween(head.next, j - 1)
        head.next.next = head
        head.next = successor
        return root    
    }
    
    let successor = null
    if (m <= 1)
        return _reverseBetween(head, n)
    
    head.next = reverseBetween(head.next, m - 1, n - 1)
    return  head
};
```

## 23. Merge k Sorted Lists
```javascript
// Time - O(mn) 
// Space - O(1)
var mergeKLists = function(lists) {
    if (!lists.length) return null
    
    const dummy = new ListNode(NaN)
    let curr = dummy
    
    let min = null
    let index = null

    while (true) {
        for (let i = 0; i < lists.length; i++) {
            if (!lists[i]) continue
            
            if (!min || lists[i].val < min.val) {
                min = lists[i]
                index = i
            }
        }
        
        if (!min) break
        
        let next = min.next
        min.next = null
        
        curr.next = min
        curr = curr.next
        
        min = null
        lists[index] = next
    }
    
    return dummy.next
};

// Time - O(n log n)
// Space - O(log n)
const merge = (l1, l2) => {
    if (!l1) return l2
    if (!l2) return l1
    
    const dummy = new ListNode(NaN)
    let curr = dummy
    
    while (l1 && l2) {
        if (l1.val < l2.val) {
            curr.next = l1
            l1 = l1.next
        } else {
            curr.next = l2
            l2 = l2.next
        }
        curr = curr.next
    }
    
    curr.next = l1 ? l1 : l2    
    return dummy.next
}

var mergeKLists = function(lists) {
    const _mergeKLists = (lists) => {
        if (lists.length < 1) return null
        return merge(lists.pop(), _mergeKLists(lists))
    }
    if (!lists.length) return null
    return _mergeKLists(lists)
};

// O(nk)
// O(1)
const merge = (l1, l2) => {
    if (!l1) return l2
    if (!l2) return l1
    
    const dummy = new ListNode(NaN)
    let curr = dummy
    
    while (l1 && l2) {
        if (l1.val < l2.val) {
            curr.next = l1
            l1 = l1.next
        } else {
            curr.next = l2
            l2 = l2.next
        }
        curr = curr.next
    }
    
    curr.next = l1 ? l1 : l2    
    return dummy.next
}

var mergeKLists = function(lists) {
    if (!lists.length) return null
    
    let result = null
    while (lists.length)
        result = merge(result, lists.pop())
    
    return result
};

// Time - O(n log k)
// Space - O(k)
class Heap {
    constructor(elements, sortBy) {
        this.elements = elements
        this.sortBy = sortBy
        this.heapify()
    }
    
    heapify() {
        if (!this.elements.length) return
        
        for (let i = Math.floor(this.elements.length / 2) - 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    peek() {
        return this.elements[0]
    }
    
    insert(val) {
        this.elements.push(val)
        this.siftUp()
    }
  
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown(0)
        
        return element
    }
    
    siftUp() {
        let childIndex = this.elements.length - 1
        let parentIndex = this.parentIndex(childIndex)
        
        while (childIndex > 0 && this.sortBy(this.elements[childIndex], this.elements[parentIndex])) {
            let temp = this.elements[parentIndex]
            this.elements[parentIndex] = this.elements[childIndex]
            this.elements[childIndex] = temp
            
            childIndex = parentIndex
            parentIndex = this.parentIndex(childIndex) 
        }
    }
    
    siftDown(index) {
        let parentIndex = index
        while (true) {
            let leftIndex = this.leftChildIndex(parentIndex)
            let rightIndex = this.rightChildIndex(parentIndex)
            let candidate = parentIndex
            
            if (leftIndex < this.elements.length && 
                this.sortBy(this.elements[leftIndex], this.elements[candidate]))
                candidate = leftIndex
            
            if (rightIndex < this.elements.length && 
                this.sortBy(this.elements[rightIndex], this.elements[candidate]))
                candidate = rightIndex
            
            if (parentIndex === candidate) return
            
            let temp = this.elements[parentIndex]
            this.elements[parentIndex] = this.elements[candidate]
            this.elements[candidate] = temp
          
            parentIndex = candidate
        }
    }
    
    leftChildIndex(parentIndex) {
        return 2 * parentIndex + 1
    }
    
    rightChildIndex(parentIndex) {
        return 2 * parentIndex + 2
    }
    
    parentIndex(childIndex) {
        return Math.floor((childIndex - 1) / 2)
    }
}

class MergeNode {
    constructor(node, index) {
        this.node = node
        this.index = index
    }
}

var mergeKLists = function(lists) {
    if (!lists.length) return null
    
    const heap = new Heap([], (a, b) => a.node.val < b.node.val)
    const dummy = new ListNode(NaN)
    let curr = dummy
    
    for (let i = 0; i < lists.length; i++) {
        if (!lists[i]) continue
        
        const mergeNode = new MergeNode(lists[i], i)
        heap.insert(mergeNode)
    }
    
    while (heap.elements.length) {
        let { node, index } = heap.remove()
        
        let next = node.next
        node.next = null
        
        curr.next = node
        curr = curr.next
        lists[index] = next
        
        if (!next) continue
        const mergeNode = new MergeNode(lists[index], index)
        heap.insert(mergeNode)
    }
    
    return dummy.next
};
```

## 1046. Last Stone Weight
```javascript


// O(n log n)
class Heap {
    constructor(elements, sortBy) {
        this.elements = elements
        this.sortBy = sortBy
        this.heapify()
    }
    
    heapify() {
        if (!this.elements.length) return
        
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    siftUp() {
        let childIndex = this.elements.length - 1
        let parentIndex = this.parentIndex(childIndex)
        
        while (childIndex > 0 && this.sortBy(this.elements[childIndex], this.elements[parentIndex])) {
            let temp = this.elements[parentIndex]
            this.elements[parentIndex] = this.elements[childIndex]
            this.elements[childIndex] = temp
            
            childIndex = parentIndex
            parentIndex = this.parentIndex(childIndex)
        }
    }
    
    siftDown(index) {
        let parentIndex = index
        while (true) {
            let leftIndex = this.leftChildIndex(parentIndex)
            let rightIndex = this.rightChildIndex(parentIndex)
            let candidate = parentIndex
            
            if (leftIndex < this.elements.length && this.sortBy(this.elements[leftIndex], this.elements[candidate]))
                candidate = leftIndex
            
            if (rightIndex < this.elements.length && this.sortBy(this.elements[rightIndex], this.elements[candidate]))
                candidate = rightIndex
            
            if (parentIndex === candidate) return
                
            let temp = this.elements[parentIndex]
            this.elements[parentIndex] = this.elements[candidate]
            this.elements[candidate] = temp
                
            parentIndex = candidate
        }
    }
    
    insert(val) {
        this.elements.push(val)
        this.siftUp()
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown(0)
        
        return element
    }
    
    peek() {
        return this.elements[0]
    }
    
    leftChildIndex(parentIndex) {
        return 2 * parentIndex + 1
    }
    
    rightChildIndex(parentIndex) {
        return 2 * parentIndex + 2
    }
    
    parentIndex(childIndex) {
        return Math.floor((childIndex - 1) / 2)
    }
}


var lastStoneWeight = function(stones) {
    const heap = new Heap(stones, (a, b) => a > b)
    
    while (heap.elements.length > 1) {
        let x = heap.remove()
        let y = heap.remove()
        
        if (x === y) continue    
        heap.insert(x - y)
    }

    return heap.peek() ? heap.peek() : 0
};
```

## 703. Kth Largest Element in a Stream
```javascript
class Heap {
    constructor(elements, capacity, sortBy) {
        this.capacity = capacity
        this.elements = elements
        this.sortBy = sortBy
        this.heapify()
    }
    
    atCapacity() {
        return this.elements.length >= this.capacity
    }
    
    heapify() {
        if (!this.elements.length) return
        
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    insert(val) {
        if (this.atCapacity() && val > this.peek())
            this.remove()
        
        if (!this.atCapacity()) {
            this.elements.push(val)
            this.siftUp()
        }
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = this.elements[0]
        this.elements[0] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    siftDown(index) {
        let parent = index ? index : 0
        while (true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
            
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent) return
            
            let temp = this.elements[candidate]
            this.elements[candidate] = this.elements[parent]
            this.elements[parent] = temp
            
            parent = candidate
        }
    }
    
    siftUp(index) {
        let child = index ? index : this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    peek() {
        return this.elements[0]
    }
    
    leftChildIndex(parentIndex) {
        return 2 * parentIndex + 1
    }
    
    rightChildIndex(parentIndex) {
        return 2 * parentIndex + 2
    }
    
    parentIndex(childIndex) {
        return Math.floor((childIndex - 1) / 2)
    }
}

var KthLargest = function(k, nums) {
    this.heap = new Heap([], k, (a, b) => a < b)
    
    for (const num of nums)
        this.heap.insert(num)
};

/** 
 * @param {number} val
 * @return {number}
 */
KthLargest.prototype.add = function(val) {
    this.heap.insert(val)
    return this.heap.peek()
};

/** 
 * Your KthLargest object will be instantiated and called as such:
 * var obj = new KthLargest(k, nums)
 * var param_1 = obj.add(val)
 */
```

## Heap Sort
```javascript
const heapSort = (elements, sortBy) => {
  const _heapify = () => {
    for (let i = Math.floor(elements.length / 2) + 1; i >= 0; i--)
      _siftDown(i)
  }
  
  const _siftDown = (index, size) => {
    let parent = index || 0
    size = size || elements.length
    
    while (true) {
      let left = _leftChildIndex(parent)
      let right = _rightChildIndex(parent)
      let candidate = parent
      
      if (left < size && sortBy(elements[left], elements[candidate]))
        candidate = left
      
      if (right < size && sortBy(elements[right], elements[candidate]))
        candidate = right
      
      if (candidate === parent) return
      [elements[candidate], elements[parent]] = [elements[parent], elements[candidate]]
      
      parent = candidate
    }
  }
  
  const _leftChildIndex = index => 2 * index + 1
  const _rightChildIndex = index => 2 * index + 2
  
  if (elements.length <= 1) return elements
  sortBy = sortBy || ((a, b) => a > b)
  _heapify()
  
  for (let i = elements.length - 1; i > 0; i--) {
    [elements[0], elements[i]] = [elements[i], elements[0]]
    _siftDown(0, i)
  }
}


const t = [1, 6, 4, 5, 3, 8, 7, 9, 8, 7, 3]
heapSort(t)
console.log(t)
```

## 973. K Closest Points to Origin
```javascript
// O(n)
// O(1)
var kClosest = function(points, K) {
    const distance = (point) => {
        let [x, y] = point
        return (x ** 2) + (y ** 2)
    }
    
    const quickSelect = (elements, K, left, right) => {
        if (left >= right) return
        
        left = left || 0
        right = right || elements.length - 1
        
        let randomIndex = random(left, right)
        let temp = elements[right]
        elements[right] = elements[randomIndex]
        elements[randomIndex] = temp
        
        let pivot = partition(elements, left, right)
        
        if (pivot === K) return 
        
        if (pivot > K) {
            return quickSelect(elements, K, left, pivot - 1)
        } else {
            return quickSelect(elements, K, pivot + 1, right)
        }
    }
    
    const random = (min, max) => Math.floor(Math.random() * (max - min + 1)) + min
    
    const partition = (elements, left, right) => {
        let pivot = right
        let i = left - 1
        
        for (let j = left; j < right; j++) {
            if (distance(elements[j]) <= distance(elements[pivot])) {
                i++
                
                let temp = elements[i]
                elements[i] = elements[j]
                elements[j] = temp
            }
        }
        
        i++
        
        let temp = elements[i]
        elements[i] = elements[pivot]
        elements[pivot] = temp
        
        return i
    }
    
    if (!points.length) return []
    if (!K) return []
    
    quickSelect(points, K)
    return points.slice(0, K)
};

// O(n log k)
// O(k)
class Heap {
    constructor(elements, capacity, sortBy) {
        this.elements = elements
        this.capacity = capacity
        this.sortBy = sortBy
        
        this.heapify()
    }
    
    heapify() {
        if (!this.elements.length) return
        
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    siftUp() {
        let childIndex = this.elements.length - 1
        let parentIndex = this.parentIndex(childIndex)
        
        while (childIndex > 0 && this.sortBy(this.elements[childIndex], this.elements[parentIndex])) {
            let temp = this.elements[parentIndex]
            this.elements[parentIndex] = this.elements[childIndex]
            this.elements[childIndex] = temp
            
            childIndex = parentIndex
            parentIndex = this.parentIndex(childIndex)
        }
    }
    
    siftDown(index) {
        let parentIndex = index
        while (true) {
            let leftIndex = this.leftChildIndex(parentIndex)
            let rightIndex = this.rightChildIndex(parentIndex)
            let candidate = parentIndex
            
            if (leftIndex < this.elements.length && this.sortBy(this.elements[leftIndex], this.elements[candidate]))
                candidate = leftIndex
            
            if (rightIndex < this.elements.length && this.sortBy(this.elements[rightIndex], this.elements[candidate]))
                candidate = rightIndex
            
            if (parentIndex === candidate) return
                
            let temp = this.elements[parentIndex]
            this.elements[parentIndex] = this.elements[candidate]
            this.elements[candidate] = temp
                
            parentIndex = candidate
        }
    }
    
    atCapacity() {
        return this.elements.length >= this.capacity
    }
    
    insert(val) {
        if (this.atCapacity() && this.sortBy(this.peek(), val))
            this.remove()
        
        if (!this.atCapacity()) {
            this.elements.push(val)
            this.siftUp()   
        }
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown(0)
        
        return element
    }
    
    peek() {
        return this.elements[0]
    }
    
    leftChildIndex(parentIndex) {
        return 2 * parentIndex + 1
    }
    
    rightChildIndex(parentIndex) {
        return 2 * parentIndex + 2
    }
    
    parentIndex(childIndex) {
        return Math.floor((childIndex - 1) / 2)
    }
}

var kClosest = function(points, K) {
    const distance = (p1, p2) => {
        let [x1, y1] = p1
        let [x2, y2] = p2
        
        let a = ((x2 - x1) ** 2)
        let b = ((y2 - y1) ** 2)
        return Math.sqrt(a + b)
    }
    
    if (!points.length) return []
    if (!K) return []
    
    const heap = new Heap([], K, (a, b) => distance(a, [0, 0]) >= distance(b, [0, 0]))

    for (let point of points)
        heap.insert(point)
    
    return heap.elements
};
```

## 451. Sort Characters By Frequency
```javascript
class Heap {
    constructor(elements, sortBy) {
        this.elements = elements
        this.sortBy = sortBy
        
        if (this.elements.length)
           this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    siftDown(index) {
        let parent = index || 0
        while (true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
            
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent) return
            
            let temp = this.elements[candidate]
            this.elements[candidate] = this.elements[parent]
            this.elements[parent] = temp
            
            parent = candidate
        }
    }
    
    siftUp(index) {
        let child = index || this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp  
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    insert(val) {
        this.elements.push(val)
        this.siftUp()
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    leftChildIndex(index) {
        return 2 * index + 1
    }
    
    rightChildIndex(index) {
        return 2 * index + 2
    }
    
    parentIndex(index) {
        return Math.floor((index - 1) / 2)
    }
}

const frequencies = s => {
    return s.split('').reduce((result, element) => {
        result[element] = 1 + (result[element] || 0)
        return result
    }, {})
}

var frequencySort = function(s) {
    if (!s.length) return s
    
    const freqs = Object.entries(frequencies(s))
    const heap = new Heap(freqs, (a, b) => {
        if (a[1] === b[1]) {
            return a[0] < b[0]
        }
            
        return a[1] > b[1]
    })
    
    const result = []
    
    while (heap.elements.length) {
        let [char, count] = heap.remove()
        while (count--)
            result.push(char)
    }
    
    return result.join('')
};
```

## 347. Top K Frequent Elements
```javascript
const frequencies = nums => {
    return nums.reduce((result, num) => {
        result[num] = 1 + (result[num] || 0)
        return result
    }, {})
}

var topKFrequent = function(nums, k) {
    const buckets = []
    for (let [key, value] of Object.entries(frequencies(nums))) {
        key = +key
        
        if (!buckets[value]) {
            buckets[value] = [key]
            continue
        }
        buckets[value].push(key)
    }

    const result = []
    for (let i = buckets.length - 1; i >= 0; i--) {
        if (result.length >= k) break

        if (!buckets[i]) continue
        
        for (let num of buckets[i])
            result.push(num)        
    }

    return result    
};
```

## 215. Kth Largest Element in an Array
```javascript
// Heap
class Heap {
    constructor(elements, capacity, sortBy) {
        this.elements = elements
        this.capacity = capacity
        this.sortBy = sortBy
        
        if (this.elements.length)
            this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    siftDown(index) {
        let parent = index
        while (true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
            
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent) return
            
            let temp = this.elements[candidate]
            this.elements[candidate] = this.elements[parent]
            this.elements[parent] = temp
            
            parent = candidate
        }
    }
    
    siftUp(index) {
        let child = index
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    atCapacity() {
        return this.elements.length >= this.capacity
    }
    
    insert(val) {
        if (this.atCapacity() && this.sortBy(this.peek(), val))
           this.remove()
            
        if (!this.atCapacity()) {
            this.elements.push(val)
            this.siftUp(this.elements.length - 1)
        }
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown(0)
        
        return element
    }
    
    peek() {
        return this.elements[0]
    }
    
    leftChildIndex(index) {
        return 2 * index + 1
    }
    
    rightChildIndex(index) {
        return 2 * index + 2
    }
    
    parentIndex(index) {
        return Math.floor((index - 1) / 2)
    }
}

var findKthLargest = function(nums, k) {
    const heap = new Heap([], k, (a, b) => a < b)
    
    for (const num of nums)
        heap.insert(num)

    return heap.peek()
};

// Quick Select
var findKthLargest = function(nums, k) {
    const index = quickSelect(nums, 0, nums.length - 1, nums.length - k)
    return nums[index]
};

const quickSelect = (nums, start, end, k) => {
    if (start >= end) return start
    
    let randomIndex = random(start, end)
    let temp = nums[randomIndex]
    nums[randomIndex] = nums[end]
    nums[end] = temp
    
    let pivot = partition(nums, start, end)
    if (pivot === k) return pivot
    
    if (pivot > k) {
        return quickSelect(nums, start, pivot - 1, k)
    } else {
        return quickSelect(nums, pivot + 1, end, k)
    }
}

const partition = (nums, start, end) => {
    let pivot = nums[end]
    let i = start - 1
    for (let j = start; j < end; j++) {
        if (nums[j] <= pivot) {
            i++
            let temp = nums[i]
            nums[i] = nums[j]
            nums[j] = temp
        }
    }

    i++
    let temp = nums[i]
    nums[i] = nums[end]
    nums[end] = temp
    return i
}

const random = (min, max) => {
    return Math.floor(Math.random() * (max - min + 1)) + min
}

// Counting Sort
var findKthLargest = function(nums, k) {
    let max = null
    let min = null
    
    for (const num of nums) {
        max = Math.max(num, max || -Number.MAX_VALUE)
        min = Math.min(num, min || Number.MAX_VALUE)
    }
    
    let offset = min >= 0 ? 0 : Math.abs(min)
    
    const counts = Array(max + offset + 1).fill(0)
    for (const num of nums)
        counts[num + offset]++
    
    const result = []
    for (let i = 0; i < counts.length; i++)
        while (counts[i]--)
            result.push(i - offset)
    
    return result[result.length - k]
};
```

## 692. Top K Frequent Words
```javascript
// O(n log n)
// O(n)
const frequencies = arr => {
    return arr.reduce((result, ele) => {
        result[ele] = 1 + (result[ele] || 0)
        return result
    }, {})
}

var topKFrequent = function(words, k) {
    return Object
        .entries(frequencies(words))
        .sort(([wordA, freqA], [wordB, freqB]) => {
            if (freqA === freqB) return wordA < wordB ? -1 : 1
            return freqB - freqA
        })
        .slice(0, k)
        .map(([word, _]) => word)
};
```

## 442. Find All Duplicates in an Array
```javascript
// Counting Sort
var findDuplicates = function(nums) {
    const counts = Array(nums.length + 1).fill(0)
    
    for (const num of nums)
        counts[num]++
    
    const result = []
    for (let i = 0; i < counts.length; i++)
        if (counts[i] > 1)
            result.push(i)
    
    return result
};

// Comparative Sort
var findDuplicates = function(nums) {
    nums = nums.sort((a, b) => a - b)
    
    let result = []
    for (let i = 1; i < nums.length; i++) {
        if (nums[i] === nums[i-1])
            result.push(nums[i])
    }
    return result
};

// Set
var findDuplicates = function(nums) {
    const seen = new Set()
    const result = []
    
    for (const num of nums) {
        if (seen.has(num))
            result.push(num)  
        else
            seen.add(num)
    }
    return result
};

// Mutate
var findDuplicates = function(nums) {
    const result = []
    
    for (const num of nums) {
        const index = Math.abs(num) - 1
        const val = -1 * nums[index]
        
        if (nums[index] < 0) {
            result.push(index + 1)
            continue
        }
        
        nums[index] = val
    }
    
    return result
};
```

## 153. Find Minimum in Rotated Sorted Array
```javascript
// O(n)
var findMin = function(nums) {
    return Math.min(...nums)
};

// O(log n)
var findMin = function(nums) {
    let left = 0
    let right = nums.length - 1
    
    while (left < right) {
        let mid = Math.floor((right - left) / 2) + left
        
        if (nums[mid] > nums[right]) {
            left = mid + 1
        } else {
            right = mid
        }
    }
    
    return nums[left]
};
```

## 33. Search in Rotated Sorted Array
```javascript
const rotationPoint = nums => {
    let left = 0
    let right = nums.length - 1
    
    while (left < right) {
        let mid = Math.floor((right - left) / 2) + left
        
        if (nums[right] >= nums[mid]) {
            right = mid
        } else {
            left = mid + 1
        }
    }
    return left
}

const binarySearch = (nums, left, right, target) => {
    while (left <= right) {
        let mid = Math.floor((right - left) / 2) + left
        
        if (nums[mid] === target) 
            return mid
        
        if (nums[mid] > target) {
            right = mid - 1
        } else {
            left = mid + 1
        }
    }
    return -1
}

var search = function(nums, target) {
    const rotationIndex = rotationPoint(nums)
    
    if (nums[rotationIndex] === target)
        return rotationIndex

    if (nums[0] <= target && target <= nums[rotationIndex - 1]) {
        return binarySearch(nums, 0, rotationIndex - 1, target)
    } else {
        return binarySearch(nums, rotationIndex + 1, nums.length - 1, target)
    }
};
```

## 378. Kth Smallest Element in a Sorted Matrix
```javascript
class Heap {
    constructor(elements, capacity, sortBy) {
        this.elements = elements
        this.capacity = capacity
        this.sortBy = sortBy
        
        if (this.elements.length)
            this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    siftDown(index) {
        let parent = index || 0
        while (true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
            
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent) return
            
            let temp = this.elements[candidate]
            this.elements[candidate] = this.elements[parent]
            this.elements[parent] = temp
            
            parent = candidate
        }
    }
    
    siftUp(index) {
        let child = index || this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    atCapacity() {
        return this.elements.length >= this.capacity
    }
    
    insert(val) {
        if (this.atCapacity() && this.sortBy(val, this.elements[0]))
            this.remove()
        
        if (!this.atCapacity()) {
            this.elements.push(val)
            this.siftUp()
        } 
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    leftChildIndex(index) {
        return 2 * index + 1
    }
    
    rightChildIndex(index) {
        return 2 * index + 2
    }
    
    parentIndex(index) {
        return Math.floor((index - 1) / 2)
    }
}

class Node {
    constructor(val, row, col) {
        this.val = val
        this.row = row
        this.col = col
    }
}

var kthSmallest = function(matrix, k) {
    const heap = new Heap([], k, (a, b) => a.val < b.val)
    
    for (let row = 0; row < matrix.length; row++) {
        const node = new Node(matrix[row][0], row, 0)
        heap.insert(node)
    }
    
    for (let i = 1; i < k; i++) {
        const node = heap.remove()
        
        if (node.col + 1 < matrix[0].length) {
            const nextNode = new Node(matrix[node.row][node.col + 1], node.row, node.col + 1)
            heap.insert(nextNode)    
        }
    }
    
    return heap.elements[0].val
};
```

## 373. Find K Pairs with Smallest Sums
```javascript
class Heap {
    constructor(elements, capacity, sortBy) {
        this.elements = elements
        this.capacity = capacity
        this.sortBy = sortBy
        
        if (this.elements.length)
            this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    siftDown(index) {
        let parent = index || 0
        while (true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
            
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (parent === candidate) return
            
            let temp = this.elements[parent]
            this.elements[parent] = this.elements[candidate]
            this.elements[candidate] = temp
            
            parent = candidate
        }
    }
    
    siftUp(index) {
        let child = index || this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    atCapacity() {
        return this.elements.length >= this.capacity
    }
    
    insert(val) {
        if (this.atCapacity() && this.sortBy(val, this.elements[0]))
            this.remove()
        
        if (!this.atCapacity()) {
            this.elements.push(val)
            this.siftUp()
        }
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    leftChildIndex(index) {
        return 2 * index + 1
    }
    
    rightChildIndex(index) {
        return 2 * index + 2
    }
    
    parentIndex(index) {
        return Math.floor((index - 1) / 2)
    }
}

class Node {
    constructor(sum, leftVal, rightVal, rightPos) {
        this.sum = sum
        this.leftVal = leftVal
        this.rightVal = rightVal
        this.rightPos = rightPos
    }
}

var kSmallestPairs = function(nums1, nums2, k) {
    if (!nums1.length || !nums2.length) return []
    
    const heap = new Heap([], k, (a, b) => a.sum < b.sum)
    const result = []
    
    for (let i = 0; i < nums1.length; i++) {
        const node = new Node(nums1[i] + nums2[0], nums1[i], nums2[0], 0)
        heap.insert(node)
    }
    
    while (k > 0) {
        const node = heap.remove()
        
        if (!node) break
        result.push([node.leftVal, node.rightVal])
        
        if (node.rightPos < nums2.length - 1) {
            const newNode = new Node(node.leftVal + nums2[node.rightPos + 1], 
                                     node.leftVal, 
                                     nums2[node.rightPos + 1], 
                                     node.rightPos + 1)
            heap.insert(newNode)
        }
        k--
    }
    
    return result
};
```

## 621. Task Scheduler
```javascript
// Sorting
var leastInterval = function(tasks, n) {
    const counts = Array(26).fill(0)
    for (const task of tasks)
        counts[task.charCodeAt(0) - 'A'.charCodeAt(0)]++
    
    counts.sort((a, b) => a - b)
    
    let time = 0
    while (counts[25] > 0) {
        for (let i = 0; i <= n; i++) {
            if (counts[25] === 0)
                break
            
            if (i < 26 && counts[25 - i] > 0) {
                counts[25 - i]--
            }
            
            time++
        }
        counts.sort((a, b) => a - b)
    }
    return time
};

// Heap
class Heap {
    constructor(elements, sortBy) {
        this.elements = elements
        this.sortBy = sortBy
        
        if (this.elements.length)
            this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    siftDown(index) {
        let parent = index || 0
        while (true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
                
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent) return
            
            let temp = this.elements[parent]
            this.elements[parent] = this.elements[candidate]
            this.elements[candidate] = temp
            
            parent = candidate
        }
    }
    
    siftUp(index) {
        let child = index || this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    insert(val) {
        this.elements.push(val)
        this.siftUp()
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    leftChildIndex(index) {
        return 2 * index + 1
    }
    
    rightChildIndex(index) {
        return 2 * index + 2
    }
    
    parentIndex(index) {
        return Math.floor((index - 1) / 2)
    }
    
    isEmpty() {
        return this.elements.length === 0
    }
}

var leastInterval = function(tasks, n) {
    const counts = Array(tasks.length).fill(0)
    for (const task of tasks)
        counts[task.charCodeAt(0) - 'A'.charCodeAt(0)]++
    
    const heap = new Heap(counts.filter(count => count), (a, b) => a > b)
    
    let cycles = 0
    
    while (!heap.isEmpty()) {
        let tasksCompleted = [] 
        for (let i = 0; i <= n; i++) {            
            let task = heap.remove() - 1
            if (task > 0) tasksCompleted.push(task)    
            cycles++
            
            if (heap.isEmpty() && !tasksCompleted.length) break
        }
        
        for (const task of tasksCompleted)
            heap.insert(task)
    }
    
    return cycles
};

```

## 767. Reorganize String
```javascript
class Heap {
    constructor(elements, sortBy) {
        this.elements = elements
        this.sortBy = sortBy
        
        if (this.elements.length)
            this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
            
    }
    
    siftDown(index) {
        let parent = index || 0
        while (true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
            
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent) return
            
            let temp = this.elements[parent]
            this.elements[parent] = this.elements[candidate]
            this.elements[candidate] = temp
            
            parent = candidate
        }
    }
    
    siftUp(index) {
        let child = index || this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    insert(val) {
        this.elements.push(val)
        this.siftUp()
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    leftChildIndex(index) {
        return 2 * index + 1
    }
    
    rightChildIndex(index) {
        return 2 * index + 2
    }
    
    parentIndex(index) {
        return Math.floor((index - 1) / 2)
    }
    
    isEmpty() {
        return this.elements.length === 0
    }
}

const counts = str => {
    return str.split('').reduce((result, ele) => {
        result[ele] = 1 + (result[ele] || 0)
        return result
    }, {})
}

var reorganizeString = function(S) {
    const charCounts = Object.entries(counts(S))
    const heap = new Heap(charCounts, (a, b) => a[1] > b[1])
    const result = []
    
    while (!heap.isEmpty()) {
        let list = []
        
        for (let i = 0; i < 2; i++) {
            if (!heap.isEmpty()) {
                let [char, charCount] = heap.remove()

                if (result[result.length - 1] === char) 
                    return ""

                result.push(char)

                if (--charCount > 0)
                    list.push([char, charCount])
            }
            
            if (heap.isEmpty() && !list.length) break
        }
        
        for (const char of list)
            heap.insert(char)
    }
    return result.join('')
};
```

## 358. Rearrange String k Distance Apart
```javascript
class Heap {
    constructor(elements, sortBy) {
        this.elements = elements
        this.sortBy = sortBy
        
        if (this.elements.length)
            this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
            
    }
    
    siftDown(index) {
        let parent = index || 0
        while (true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
            
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent) return
            
            let temp = this.elements[parent]
            this.elements[parent] = this.elements[candidate]
            this.elements[candidate] = temp
            
            parent = candidate
        }
    }
    
    siftUp(index) {
        let child = index || this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    insert(val) {
        this.elements.push(val)
        this.siftUp()
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    leftChildIndex(index) {
        return 2 * index + 1
    }
    
    rightChildIndex(index) {
        return 2 * index + 2
    }
    
    parentIndex(index) {
        return Math.floor((index - 1) / 2)
    }
    
    isEmpty() {
        return this.elements.length === 0
    }
}

const counts = str => {
    return str.split('').reduce((result, ele) => {
        result[ele] = 1 + (result[ele] || 0)
        return result
    }, {})
}

var rearrangeString = function(s, k) {
    const strCounts = Object.entries(counts(s))
    const heap = new Heap(strCounts, (a, b) => a[1] > b[1])
    const result = []
    const queue = []
    
    while (!heap.isEmpty()) {
        const curr = heap.remove()
        result.push(curr[0])
        curr[1]--
        queue.push(curr)
        
        if (queue.length < k) continue
        
        let element = queue.shift()
        if (element[1] > 0) heap.insert(element)
    }

    return result.length == s.length ? result.join('') : ''
};
```

## 1054. Distant Barcodes
```javascript
class Heap {
    constructor(elements, sortBy) {
        this.elements = elements
        this.sortBy = sortBy
        
        if (this.elements.length)
            this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    siftDown(index) {
        let parent = index || 0
        while (true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
                
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent) return
            
            let temp = this.elements[parent]
            this.elements[parent] = this.elements[candidate]
            this.elements[candidate] = temp
            
            parent = candidate
        }
    }
    
    siftUp(index) {
        let child = index || this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    insert(val) {
        this.elements.push(val)
        this.siftUp()
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    leftChildIndex(index) {
        return 2 * index + 1
    }
    
    rightChildIndex(index) {
        return 2 * index + 2
    }
    
    parentIndex(index) {
        return Math.floor((index - 1) / 2)
    }
    
    isEmpty() {
        return this.elements.length === 0
    }
}

const counts = arr => {
    return arr.reduce((result, ele) => {
        result[ele] = 1 + (result[ele] || 0)
        return result
    }, {})
}

var rearrangeBarcodes = function(barcodes) {
    if (barcodes.length <= 1) return barcodes
    
    const barcodeCounts = Object.entries(counts(barcodes))
    const heap = new Heap(barcodeCounts, (a, b) => a[1] > b[1])
    const result = []
    
    while (!heap.isEmpty()) {
        const b1 = heap.remove()
        const b2 = heap.remove()
        
        if (b1) {
            result.push(+b1[0])
            if (b1) b1[1]--
            if (b1[1] > 0) heap.insert(b1)
        }
        
        if (b2) {
            result.push(+b2[0])
            if (b2) b2[1]--
            if (b2[1] > 0) heap.insert(b2)
        }
    }
    
    return result
};

// O(n)
var rearrangeBarcodes = function(barcodes) {
    const counts = Array(10001).fill(0)
    
    let maxCount = 0
    let maxCode = 0
    
    for (const barcode of barcodes) {
        maxCount = Math.max(maxCount, ++counts[barcode])
        maxCode = maxCount === counts[barcode] ? barcode : maxCode
    }
    
    let pos = 0
    
    for (let i = 0; i < counts.length; i++) {
        let code = i === 0 ? maxCode : i
        
        while (counts[code]-- > 0) {
            barcodes[pos] = code
            pos = pos + 2 < barcodes.length ? pos + 2 : 1
        }
    }
    
    return barcodes
};
```

## 253. Meeting Rooms II
```javascript
class Heap {
    constructor(elements, sortBy) {
        this.elements = elements
        this.sortBy = sortBy
        
        if (this.elements.length)
            this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    siftDown(index) {
        let parent = index || 0
        while (true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
            
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent) return
            
            let temp = this.elements[candidate]
            this.elements[candidate] = this.elements[parent]
            this.elements[parent] = temp
            
            parent = candidate
        }
    } 
    
    siftUp(index) {
        let child = index || this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    insert(val) {
        this.elements.push(val)
        this.siftUp()
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    leftChildIndex(index) {
        return 2 * index + 1
    }
    
    rightChildIndex(index) {
        return 2 * index + 2
    }
    
    parentIndex(index) {
        return Math.floor((index - 1) / 2)
    }
}

var minMeetingRooms = function(intervals) {
    if (!intervals.length) return 0
    
    intervals.sort((a, b) => a[0] - b[0])
    const heap = new Heap([intervals[0]], (a, b) => a[1] < b[1])
    
    for (let interval of intervals.slice(1)) {
        let earliest = heap.remove()
        
        if (interval[0] >= earliest[1]) {
            earliest[1] = interval[1]
        } else {
            heap.insert(interval)
        }   
        heap.insert(earliest)
    }
    return heap.elements.length
};
```

## 263. Ugly Number
```javascript
var isUgly = function(num) {
    if (num <= 0) return false
    while (num % 2 == 0) num /= 2
    while (num % 3 == 0) num /= 3
    while (num % 5 == 0) num /= 5
    return num == 1
};
```

## 264. Ugly Number II
```javascript
class Heap {
    constructor(elements, sortBy) {
        this.elements = elements
        this.sortBy = sortBy
        
        if (this.elements.length)
            this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    siftDown(index) {
        let parent = index || 0
        while (true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
            
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent) return
            
            let temp = this.elements[candidate]
            this.elements[candidate] = this.elements[parent]
            this.elements[parent] = temp
            
            parent = candidate
        }
    } 
    
    siftUp(index) {
        let child = index || this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    insert(val) {
        this.elements.push(val)
        this.siftUp()
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    leftChildIndex(index) {
        return 2 * index + 1
    }
    
    rightChildIndex(index) {
        return 2 * index + 2
    }
    
    parentIndex(index) {
        return Math.floor((index - 1) / 2)
    }
}

const cache = { 1: 1 }
var nthUglyNumber = function(n) {
    if (cache[n]) return cache[n]
    
    const heap = new Heap([1], (a, b) => a < b)
    const primes = [2, 3, 5]
    const seen = new Set()
    
    for (let i = n; i > 0; i--) {
        const top = heap.remove()
        cache[n] = top
        
        for (const num of primes) {
            const currNum = top * num
            if (!seen.has(currNum)) {
                heap.insert(currNum)
                seen.add(currNum)
            }
        }
    }
    
    return cache[n]
};
```

## 313. Super Ugly Number
```javascript
var nthSuperUglyNumber = function(n, primes) {
    if (n === 1) return 1
    
    const heap = new Heap([1], (a, b) => a < b)
    const seen = new Set()
    let last = 0
    
    for (let i = n; i > 0; i--) {
        const top = heap.remove()
        last = top
        
        for (const num of primes) {
            const currNum = top * num
            if (!seen.has(currNum)) {
                heap.insert(currNum)
                seen.add(currNum)
            }
        }
    }
    
    return last
};

class Heap {
    constructor(elements, sortBy) {
        this.elements = elements
        this.sortBy = sortBy
        
        if (this.elements.length)
            this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    siftDown(index) {
        let parent = index || 0
        while (true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
            
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent) return
            
            let temp = this.elements[candidate]
            this.elements[candidate] = this.elements[parent]
            this.elements[parent] = temp
            
            parent = candidate
        }
    } 
    
    siftUp(index) {
        let child = index || this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    insert(val) {
        this.elements.push(val)
        this.siftUp()
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    leftChildIndex(index) {
        return 2 * index + 1
    }
    
    rightChildIndex(index) {
        return 2 * index + 2
    }
    
    parentIndex(index) {
        return Math.floor((index - 1) / 2)
    }
}
```

## 787. Cheapest Flights Within K Stops
```javascript
// BFS
var findCheapestPrice = function(n, flights, src, dst, K) {
    const graph = {}
    
    for (const [vertex, neighbor, weight] of flights) {
        if (!graph[vertex]) {
            graph[vertex] = [[neighbor, weight]]
        } else {
            graph[vertex].push([neighbor, weight])
        }
    }

    const queue = [[src, 0, 0]]
    let minCost = Number.MAX_VALUE
    
    while (queue.length) {
        const [node, stops, costSoFar] = queue.shift()
        
        if (node === dst) {
            minCost = Math.min(minCost, costSoFar)
            continue
        }
        
        if (stops > K || costSoFar > minCost)
            continue
        
        if (!graph[node])
            continue
        
        for (const [neighbor, cost] of graph[node]) {
            queue.push([neighbor, stops + 1, costSoFar + cost])
        }
    }
    
    return minCost !== Number.MAX_VALUE ? minCost : -1
};

// Dijkstra's
class Heap {
    constructor(elements, sortBy) {
        this.elements = elements
        this.sortBy = sortBy
        
        if (this.elements.length)
            this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    insert(val) {
        this.elements.push(val)
        this.siftUp()
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    siftUp(index) {
        let child = index || this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    siftDown(index) {
        let parent = index || 0
        while(true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
            
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent)
                return
            
            let temp = this.elements[candidate]
            this.elements[candidate] = this.elements[parent]
            this.elements[parent] = temp
            
            parent = candidate
        }
    }
    
    leftChildIndex(parent) {
        return 2 * parent + 1
    }
    
    rightChildIndex(parent) {
        return 2 * parent + 2
    }
    
    parentIndex(child) {
        return Math.floor((child - 1) / 2)
    }
}

var findCheapestPrice = function(n, flights, src, dst, K) {
    const graph = {}
    for (const [vertex, neighbor, weight] of flights) {
        if (!graph[vertex]) {
            graph[vertex] = [[neighbor, weight]]
        } else {
            graph[vertex].push([neighbor, weight])
        }
    }
    
    const heap = new Heap([], (a, b) => a[1] < b[1])
    heap.insert([src, 0, 0])
    
    while (heap.elements.length) {
        const [vertex, costSoFar, stops] = heap.remove()
        
        if (vertex === dst)
            return costSoFar
        
        if (stops > K)
            continue
        
        if (!graph[vertex]) continue
        
        for (const [neighbor, weight] of graph[vertex]) {
            heap.insert([neighbor, costSoFar + weight, stops + 1])
        }
    }
    
    return -1
};
```

## 743. Network Delay Time
```javascript
class Heap {
    constructor(elements, sortBy) {
        this.elements = elements
        this.sortBy = sortBy
        
        if (this.elements.length)
            this.heapify()
    }
    
    heapify() {
        for (let i = Math.floor(this.elements.length / 2) + 1; i >= 0; i--)
            this.siftDown(i)
    }
    
    insert(val) {
        this.elements.push(val)
        this.siftUp()
    }
    
    remove() {
        if (!this.elements.length) return null
        
        let temp = this.elements[0]
        this.elements[0] = this.elements[this.elements.length - 1]
        this.elements[this.elements.length - 1] = temp
        
        let element = this.elements.pop()
        
        this.siftDown()
        
        return element
    }
    
    siftUp(index) {
        let child = index || this.elements.length - 1
        let parent = this.parentIndex(child)
        
        while (child > 0 && this.sortBy(this.elements[child], this.elements[parent])) {
            let temp = this.elements[child]
            this.elements[child] = this.elements[parent]
            this.elements[parent] = temp
            
            child = parent
            parent = this.parentIndex(child)
        }
    }
    
    siftDown(index) {
        let parent = index || 0
        while(true) {
            let left = this.leftChildIndex(parent)
            let right = this.rightChildIndex(parent)
            let candidate = parent
            
            if (left < this.elements.length && this.sortBy(this.elements[left], this.elements[candidate]))
                candidate = left
            
            if (right < this.elements.length && this.sortBy(this.elements[right], this.elements[candidate]))
                candidate = right
            
            if (candidate === parent)
                return
            
            let temp = this.elements[candidate]
            this.elements[candidate] = this.elements[parent]
            this.elements[parent] = temp
            
            parent = candidate
        }
    }
    
    leftChildIndex(parent) {
        return 2 * parent + 1
    }
    
    rightChildIndex(parent) {
        return 2 * parent + 2
    }
    
    parentIndex(child) {
        return Math.floor((child - 1) / 2)
    }
}

var networkDelayTime = function(times, N, K) {
    const graph = {}
    for (const [vertex, neighbor, weight] of times) {
        if (!graph[vertex]) {
            graph[vertex] = [[neighbor, weight]]
        } else {
            graph[vertex].push([neighbor, weight])
        }
    }
    
    const distances = {}
    for (let i = 1; i <= N; i++)
        distances[i] = Number.MAX_VALUE
    distances[K] = 0
    
    const visited = new Set()
    
    const heap = new Heap([], (a, b) => a[1] < b[1])
    heap.insert([K, 0])
    
    while (heap.elements.length) {
        const [vertex, weight] = heap.remove()
        
        if (visited.has(vertex)) continue
        visited.add(vertex)
    
        if (!distances[vertex]) {
            distances[vertex] = weight
        } else {
            distances[vertex] = Math.min(distances[vertex], weight)
        }
        
        if (!graph[vertex]) continue
        
        for (const [neighbor, neighborWeight] of graph[vertex]) {
            if (!visited.has(neighbor) && weight + neighborWeight < distances[neighbor]) {
                heap.insert([neighbor, weight + neighborWeight])   
            }
        }
    }
    
    const max = Math.max(...Object.values(distances))
    return max === Number.MAX_VALUE ? -1 : max
};
```

## 147. Insertion Sort List
```javascript
function insertionSortList(head) {
    if (!head) return head
    
    const dummy = new ListNode(NaN)
    
    while (head) {
        let prev = dummy
        
        while (prev.next && prev.next.val < head.val)
            prev = prev.next
        
        const next = head.next
        head.next = prev.next
        prev.next = head
        head = next
    }
    
    return dummy.next
}
```

## 148. Sort List
```javascript
const listLength = head => {
    let length = 0
    
    while (head) {
        head = head.next
        length++
    }
        
    return length
}

const mergeK = (head, k) => {
    const dummy = new ListNode(NaN)
    dummy.next = head
    let curr = dummy
    let currHead = head
    
    while (currHead) {
        let l1Head = currHead
        let l1Tail = l1Head

        let i = 1
        while (l1Tail.next && i < k) {
            l1Tail = l1Tail.next
            i++
        }
            
        let l2Head = l1Tail.next
        let l2Tail = l2Head
        
        if (!l2Tail) break
        
        l1Tail.next = null

        i = 1
        while (l2Tail.next && i < k) {
            l2Tail = l2Tail.next
            i++
        }
            
        let next = l2Tail.next
        l2Tail.next = null

        const merged = merge(l1Head, l2Head)
        let mergedTail = merged

        while (mergedTail.next)
            mergedTail = mergedTail.next

        mergedTail.next = next
        
        curr.next = merged
        curr = mergedTail
        currHead = next
    }
    
    return dummy.next
}

const merge = (left, right) => {
    if (!left) return right
    if (!right) return left
    
    let dummy = new ListNode(NaN)
    let curr = dummy
    
    while (left && right) {
        if (left.val < right.val) {
            curr.next = left
            left = left.next
        } else {
            curr.next = right
            right = right.next
        }
        curr = curr.next
    }
    
    curr.next = left ? left : right
    return dummy.next
}

var sortList = function(head) {
    if (!head) return head
    
    const length = listLength(head)
    
    let steps = 1
    while (steps < length) {
        head = mergeK(head, steps)
        steps *= 2
    }
    
    return head
};
```

## 207. Course Schedule
```javascript
const buildGraph = edges => {
    const graph = {}
    
    for (const [vertex, neighbor] of edges) {
        if (!graph[vertex]) {
            graph[vertex] = [neighbor]
        } else {
            graph[vertex].push(neighbor)
        }
    }
    
    return graph
}

var canFinish = function(numCourses, prerequisites) {
    const dfs = (start) => {
        if (visiting.has(start))
            return true
        
        if (visited.has(start))
            return false
        
        visiting.add(start)
        
        let neighbors = graph[start]
        if (neighbors)
            for (const neighbor of neighbors)
                if (dfs(neighbor))
                    return true
        
        visiting.delete(start)
        visited.add(start)
        return false
    }
    
    const graph = buildGraph(prerequisites)
    const visited = new Set()
    const visiting = new Set()
    
    for (const vertex of Object.keys(graph))
        if (dfs(vertex)) return false
    
    return true
};
```

## 210. Course Schedule II
```javascript
const buildGraph = edges => {
    const graph = new Map()
    
    for (const [vertex, neighbor] of edges) {
        if (!graph.has(vertex)) {
            graph.set(vertex, [neighbor])
        } else {
            graph.get(vertex).push(neighbor)
        }
    }
    
    return graph
}

var findOrder = function(numCourses, prerequisites) {
    const dfs = start => {
        if (visiting.has(start))
            return true
        
        if (visited.has(start))
            return false
        
        visiting.add(+start)
        
        let neighbors = graph.get(start)
        if (neighbors)
            for (let neighbor of neighbors)
                if (dfs(neighbor))
                    return true
        
        visiting.delete(start)
        visited.add(start)
        order.push(start)
        return false
    }
    
    const graph = buildGraph(prerequisites)
    const visited = new Set()
    const visiting = new Set()
    const order = []
    
    for (const vertex of graph.keys())
        if (dfs(vertex))
            return []
    
    for (let i = 0; i < numCourses; i++)
        if (!visited.has(i))
            order.push(i)
    
    return order
};

// BFS
const buildGraph = edges => {
    const graph = new Map()
    
    for (const [vertex, neighbor] of edges) {
        if (!graph.has(vertex)) {
            graph.set(vertex, [neighbor])
        } else {
            graph.get(vertex).push(neighbor)
        }
    }
    
    return graph
}

const getIndegrees = edges => {
    const indegrees = new Map()
    
    for (const [vertex, neighbor] of edges) {
        if (!indegrees.has(neighbor)) {
            indegrees.set(neighbor, 1)
        } else {
            const curr = indegrees.get(neighbor)
            indegrees.set(neighbor, curr + 1)
        }
    }
    
    return indegrees
}

var findOrder = function(numCourses, prerequisites) {
    const graph = buildGraph(prerequisites)
    const indegrees = getIndegrees(prerequisites)
    const queue = []
    const order = []
    let count = 0
    
    for (let i = 0; i < numCourses; i++)
        if (!indegrees.has(i))
            queue.push(i)
    
    while (queue.length) {
        const vertex = queue.pop()
        order.push(vertex)
        count++
        
        if (graph.get(vertex)) {
            for (const neighbor of graph.get(vertex)) {
                const curr = indegrees.get(neighbor)
                indegrees.set(neighbor, curr - 1)

                if (indegrees.get(neighbor) === 0)
                    queue.push(neighbor)
            }
        }
    }
    
    return count === numCourses ? order.reverse() : []
};
```

## 582. Kill Process
```javascript
var killProcess = function(pid, ppid, kill) {
    const map = new Map()
    for (let i = 0; i < pid.length; i++) {
        if (!map.has(ppid[i])) {
            map.set(ppid[i], [pid[i]])
        } else {
            const pids = map.get(ppid[i])
            pids.push(pid[i])
        }
    }
    
    const queue = [kill]
    const result = []
    
    while (queue.length) {
        const process = queue.shift()
        result.push(process)
        
        const children = map.get(process)
        if (!children) continue
        
        for (const child of children)
            queue.push(child)
        
    }
    
    return result
};
```

## 303. Range Sum Query - Immutable
```javascript
/**
 * @param {number[]} nums
 */
var NumArray = function(nums) {
    this.prefix = nums.reduce((result, num) => {
        let last = result[result.length - 1] || 0
        result.push(last + num)
        return result
    }, [])
};

/** 
 * @param {number} i 
 * @param {number} j
 * @return {number}
 */
NumArray.prototype.sumRange = function(i, j) {
    if (i === 0)
        return this.prefix[j]
    
    return this.prefix[j] - this.prefix[i-1]
};

/** 
 * Your NumArray object will be instantiated and called as such:
 * var obj = new NumArray(nums)
 * var param_1 = obj.sumRange(i,j)
 */
```

## 280. Wiggle Sort
```javascript
var wiggleSort = function(nums) {
    nums.sort((a, b) => a - b)
    
    for (let i = 1; i < nums.length - 1; i += 2) {
        let temp = nums[i]
        nums[i] = nums[i+1]
        nums[i+1] = temp
    }
};

const isEven = num => (num & 1) === 0

const swap = (arr, i, j) => {
    let temp = arr[i]
    arr[i] = arr[j]
    arr[j] = temp
}

var wiggleSort = function(nums) {
    for (let i = 0; i < nums.length - 1; i++) {
        if (isEven(i)) {
            if (!(nums[i] < nums[i+1]))
                swap(nums, i, i+1)
        } else {
            if (!(nums[i] > nums[i+1]))
                swap(nums, i, i+1)
        }
    }
};
```

## 1057. Campus Bikes
```javascript
var assignBikes = function(workers, bikes) {
  const buckets = buildBuckets(workers, bikes)
  const result = []
  
  for (let i = 0; i < buckets.length; i++) {
      for (let j = 0; j < buckets[i].length; j++) {
          const { bike, worker } = buckets[i][j]
          if (bikes[bike] && workers[worker]) {
              workers[worker] = null
              bikes[bike] = null
              result[worker] = bike
          }
      }    
  }
      
  return result
}

const buildBuckets = (workers, bikes) => {
    let buckets = Array(2001).fill(null).map(() => [])
    for (let i = 0; i < workers.length; i++) {
        for (let j = 0; j < bikes.length; j++) {
            const dist = manhattanDist(workers[i], bikes[j])    
            buckets[dist].push({ worker: i, bike: j })
        }
    }
    
    return buckets
}

const manhattanDist = (p1, p2) => {
    return Math.abs(p1[0] - p2[0]) + Math.abs(p1[1] - p2[1])
}
```

## 1030. Matrix Cells in Distance Order
```javascript
// Counting Sort
const dist = (p1, p2) => {
    return Math.abs(p1[0] - p2[0]) + Math.abs(p1[1] - p2[1])
}

var allCellsDistOrder = function(R, C, r0, c0) {
    let buckets = Array(R + C + 1).fill(null).map(() => [])
    for (let row = 0; row < R; row++) {
        for (let column = 0; column < C; column++) {
            let cellDist = dist([row, column], [r0, c0])
            buckets[cellDist].push([row, column])
        }
    }
    
    let result = []
    for (let bucket of buckets) {
        for (let cell of bucket) {
            result.push(cell)
        }
    }
    return result
};

// BFS
var allCellsDistOrder = function(R, C, r0, c0) {
    const result = []
    const queue = []
    const visited = Array(R).fill(null).map(() => Array(C).fill(null).map(() => false))
    
    queue.push([r0, c0])
    visited[r0][c0] = true
    
    while (queue.length) {
        const curr = queue.shift()
        result.push(curr)
        
        enqueue(queue, R, C, curr[0] - 1, curr[1], visited)
        enqueue(queue, R, C, curr[0] + 1, curr[1], visited)
        enqueue(queue, R, C, curr[0], curr[1] - 1, visited)
        enqueue(queue, R, C, curr[0], curr[1] + 1, visited)
    }
    
    return result
};

const enqueue = (queue, R, C, currR, currC, visited) => {
    if (currR >= 0 && currR < R && 
        currC >= 0 && currC < C && 
        !visited[currR][currC]) {
        
        queue.push([currR, currC])
        visited[currR][currC] = true
    }    
}
```

## 853. Car Fleet
```javascript
var carFleet = function(target, position, speed) {
    const map = new Map()
    for (let i = 0; i < position.length; i++)
        map.set(position[i], speed[i])
    
    position.sort((a, b) => b - a)
    
    let time = -1
    let fleets = 0
    
    for (let i = 0; i < position.length; i++) {
        const currTime = (target - position[i]) / map.get(position[i])
        if (currTime > time) {
            time = currTime
            fleets++
        }
    }
    
    return fleets
};
```

## 56. Merge Intervals
```javascript
var merge = function(intervals) {
    if (intervals.length <= 1) return intervals
    intervals.sort((a, b) => a[0] - b[0])
    
    let result = [intervals[0]]
    
    for (let i = 1; i < intervals.length; i++) {
        let lastInterval = result[result.length - 1]
        let currInterval = intervals[i]
        
        if (lastInterval[1] >= currInterval[0]) {
            lastInterval[1] = Math.max(lastInterval[1], currInterval[1])
            result[result.length - 1] = lastInterval
            continue
        }
        
        result.push(intervals[i])
    }
    
    return result
};
```

## 524. Longest Word in Dictionary through Deleting
```javascript
// O(nm)
var findLongestWord = function(s, d) {
    let result = ""

    for (let word of d) {
        let wordIndex = 0
        for (let strIndex = 0; strIndex < s.length; strIndex++) {
            if (word[wordIndex] === s[strIndex]) 
                wordIndex++
        }
        
        
        if (wordIndex === word.length && result.length <= word.length) {
            if (result.length === word.length && result < word)
                continue
            
            result = word
        }
    }
    
    return result
};

// O(n log m)
const getIndices = str => {
    const indices = {}
    
    for (let i = 0; i < str.length; i++) {
        let char = str[i]
        if (!indices[char]) {
            indices[char] = [i]
        } else {
            indices[char].push(i)
        }
    }
    
    return indices
}

const binarySeach = (target, arr) => {
    let left = 0
    let right = arr.length - 1
    
    while (left < right) {
        let middle = Math.floor((right - left) / 2) + left
        let middleVal = arr[middle]
        
        if (middleVal > target) {
            right = middle
        } else {
            left = middle + 1
        }
    }
    return target < arr[left] ? arr[left] : null
}

var findLongestWord = function(s, d) {
    const indices = getIndices(s)
    
    let result = ""
    
    for (let word of d) {
        
        let length = 0
        let curr = -1
        for (let i = 0; i < word.length; i++) {
            let char = word[i]
            let charIndices = indices[char]
            if (!charIndices) break
            
            let index = binarySeach(curr, charIndices)
            if (index === null) break
            
            curr = index
            length++
        }
        
        if (length === word.length && result.length <= word.length) {
            if (result.length === word.length && result < word)
                continue
            
            result = word
        }
    }
    
    return result
};
```

## 969. Pancake Sorting
```javascript
var pancakeSort = function(A) {
    if (A.length <= 1) return []
    
    const result = []
    
    let max = A.length
    for (let i = 0; i < A.length; i++) {
        let maxIndex = find(A, max)
        flip(A, maxIndex)
        result.push(maxIndex + 1)
        
        flip(A, max - 1)
        result.push(max)
        
        max--
    }
    return result
};

const find = (A, target) => {
    for (let i = 0; i < A.length; i++)
        if (A[i] == target)
            return i
    
    return -1
}

const flip = (A, index) => {
    let i = 0
    let j = index
    
    while (i < j) {
        let temp = A[i]
        A[i] = A[j]
        A[j] = temp
        
        i++
        j--
    }   
}
```

## 75. Sort Colors
```javascript
// Two Pass
var sortColors = function(nums) {
    let counts = [0, 0, 0]
    
    for (let num of nums)
        counts[num]++
    
    let i = 0
    for (let j = 0; j < counts.length; j++)
        while (counts[j]--)
            nums[i++] = j
};

// One Pass
const swap = (arr, i, j) => {
    let temp = arr[i]
    arr[i] = arr[j]
    arr[j] = temp
}

var sortColors = function(nums) {
    let low = 0
    let high = nums.length - 1
    
    let i = 0
    while (i <= high) {
        if (nums[i] === 0) {
            swap(nums, i++, low++)
            continue
        }
        
        if (nums[i] == 2) {
            swap(nums, i, high--)
            continue
        }
        
        i++
    }
};
```

## 179. Largest Number
```javascript
var largestNumber = function(nums) {
    if (nums.every(num => num === 0)) 
        return "0"
    
    nums.sort((a, b) => {
        const s1 = `${a}${b}`
        const s2 = `${b}${a}`
        
        if (s1 < s2) return 1
        if (s1 > s2) return -1
        return 0
    })
    
    return nums.join('')
};
```

## 961. N-Repeated Element in Size 2N Array
```javascript
var repeatedNTimes = function(A) {
    const map = new Map()
    
    for (let a of A) {
        if (map.get(a) === undefined) {
            map.set(a, 1)
        } else {
            map.set(a, map.get(a) + 1)
        }
    }
    
    for (let [key, val] of map.entries()) {
        if (val === Math.floor(A.length / 2)) {
            return key
        }
    }
};

var repeatedNTimes = function(A) {
    A.sort((a, b) => a - b)
    
    let val = 0
    let valCount = 0
    
    for (let a of A) {
        if (a === val) {
            valCount++
            if (valCount === Math.floor(A.length / 2))
                return val
            
            continue
        }
        
        val = a
        valCount = 1
    }
};
```

## 1207. Unique Number of Occurrences
```javascript
var uniqueOccurrences = function(arr) {
    const map = {}
    
    for (let a of arr) {
        if (!map[a]) {
            map[a] = 1
        } else {
            map[a]++
        }
    }
    
    const counts = Object.values(map)
    const unique = new Set(counts)
    return unique.size === counts.length
};
```

## 760. Find Anagram Mappings
```javascript
var anagramMappings = function(A, B) {
    const map = {}
    const result = []
    
    for (let i = 0; i < B.length; i++)
        map[B[i]] = i
    
    for (let a of A)
        result.push(map[a])
    
    return result
};
```

## 1152. Analyze User Website Visit Pattern
```javascript
var sequences = (pages) => {
    let set = new Set()
    for (let i = 0; i < pages.length-2; i++) {
        for (let j = i+1; j < pages.length-1; j++) {
            for (let k = j+1; k < pages.length; k++) {
                set.add(pages[i] + '-' +  pages[j] + '-' +  pages[k])
            }
        }
    }
    return set
}

var mostVisitedPattern = function(username, timestamp, website) {
    const tuples = []
    for (let i = 0; i < username.length; i++)
        tuples.push([username[i], timestamp[i], website[i]])
    
    tuples.sort((a, b) => a[1] - b[1])
    
    const userList = {}
    for (let i = 0; i < tuples.length; i++) {
        if (!userList[tuples[i][0]])
            userList[tuples[i][0]] = []
        
        userList[tuples[i][0]].push(tuples[i][2])
    }
    
    let sequence = {}
    let max = 0
    let maxSeq = []
    
    for (let user of Object.keys(userList)) {
        let pages = userList[user]
        
        if (pages.length <= 2) continue
        
        let subsequences = sequences(pages)
        for (let s of subsequences) {
            if (!sequence[s])
                sequence[s] = 0
            
            sequence[s]++
            
            if (sequence[s] > max) {
                max = sequence[s]
                maxSeq = s
            }
            
            if (sequence[s] == max && s < maxSeq) {
                max = sequence[s]
                maxSeq = s
            }
        }
    }
    return maxSeq.split('-')
};
```

## 219. Contains Duplicate II
```javascript
var containsNearbyDuplicate = function(nums, k) {
    const map = {}
    for (let i = 0; i < nums.length; i++) {
        const num = nums[i]
        if (map[num] >= 0 && i - map[num] <= k) {
            return true
        } else {
            map[num] = i
        }
    }
    
    return false
};
```

## 435. Non-overlapping Intervals
```javascript
var eraseOverlapIntervals = function(intervals) {
    let min = 0
    
    if (intervals.length <= 1) 
        return min
    
    intervals.sort((a, b) => {
        if (a[1] !== b[1])
            return a[1] - b[1]
        
        return 0
    })
    
    let interval = intervals[0]
    for (let i = 1; i < intervals.length; i++) {
        let curr = intervals[i]
        
        if (interval[1] > curr[0]) {
            min++
            continue
        }
        
        interval = curr
    }
    
    return min
};
```

## 208. Implement Trie (Prefix Tree)
```javascript
/**
 * Initialize your data structure here.
 */

class TrieNode {
    constructor(val = "") {
        this.val = val
        this.children = {}
        this.isEnd = false
    }
}

var Trie = function() {
    this.root = new TrieNode()
};

/**
 * Inserts a word into the trie. 
 * @param {string} word
 * @return {void}
 */
Trie.prototype.insert = function(word) {
    let curr = this.root
    
    for (let char of word) {
        if (!curr.children[char])
            curr.children[char] = new TrieNode(char)
        curr = curr.children[char]
    }
    
    curr.isEnd = true
};

/**
 * Returns if the word is in the trie. 
 * @param {string} word
 * @return {boolean}
 */
Trie.prototype.search = function(word) {
    let curr = this.root
    
    for (let char of word) {
        if (!curr.children[char])
            return false
        curr = curr.children[char]
    }
    
    return curr.isEnd
};

/**
 * Returns if there is any word in the trie that starts with the given prefix. 
 * @param {string} prefix
 * @return {boolean}
 */
Trie.prototype.startsWith = function(prefix) {
    let curr = this.root
    
    for (let char of prefix) {
        if (!curr.children[char])
            return false
        curr = curr.children[char]
    }
    
    return true
};

/** 
 * Your Trie object will be instantiated and called as such:
 * var obj = new Trie()
 * obj.insert(word)
 * var param_2 = obj.search(word)
 * var param_3 = obj.startsWith(prefix)
 */
```

## 677. Map Sum Pairs
```javascript
/**
 * Initialize your data structure here.
 */

class TrieNode {
    constructor(sum = 0) {
        this.sum = sum
        this.children = {}
    }
}

var MapSum = function() {
    this.root = new TrieNode()
    this.keys = {}
};

/** 
 * @param {string} key 
 * @param {number} val
 * @return {void}
 */
MapSum.prototype.insert = function(key, val) {
    let curr = this.root
    let delta = val - (this.keys[key] || 0)
    
    this.keys[key] = val
    curr.sum += delta
    
    for (let char of key) {
        if (!curr.children[char]) {
            curr.children[char] = new TrieNode()
        }
        curr = curr.children[char]
        curr.sum += delta
    }
};

/** 
 * @param {string} prefix
 * @return {number}
 */
MapSum.prototype.sum = function(prefix) {
    let curr = this.root
    
    for (let char of prefix) {
        if (!curr.children[char])
            return 0
            
        curr = curr.children[char]
    }
    
    return curr.sum
};

/** 
 * Your MapSum object will be instantiated and called as such:
 * var obj = new MapSum()
 * obj.insert(key,val)
 * var param_2 = obj.sum(prefix)
 */
```

## 648. Replace Words
```javascript
/**
 * @param {string[]} dict
 * @param {string} sentence
 * @return {string}
 */

class TrieNode {
    constructor(val = "") {
        this.val = val
        this.children = {}
        this.isEnd = false
    }
}

class Trie {
    root = new TrieNode()

    insert(word) {
        let curr = this.root
        
        for (let char of word) {
            if (!curr.children[char]) {
                curr.children[char] = new TrieNode(char)
            }
            curr = curr.children[char]
        }
        
        curr.isEnd = true
    }

    prefix(word) {
        let curr = this.root
        let prefix = []
        
        for (let char of word) {
            if (!curr.children[char]) return null
            
            curr = curr.children[char]
            prefix.push(char)
            if (curr.isEnd) return prefix.join('')
        }
        
        if (curr.isEnd) return prefix.join('')
    }
}

var replaceWords = function(dict, sentence) {
    const trie = new Trie()
    for (let word of dict)
        trie.insert(word)
    
    const words = sentence.split(' ')
    return words.map(word => {
        let prefix = trie.prefix(word)
        if (prefix) return prefix 
        return word
    }).join(' ')
};
```

## 211. Add and Search Word - Data structure design
```javascript
/**
 * Initialize your data structure here.
 */

class TrieNode {
    constructor(val) {
        this.val = val
        this.children = {}
        this.isEnd = false
    }
}

var WordDictionary = function() {
    this.root = new TrieNode()
};

/**
 * Adds a word into the data structure. 
 * @param {string} word
 * @return {void}
 */
WordDictionary.prototype.addWord = function(word) {
    let curr = this.root
    
    for (let char of word) {
        if (!curr.children[char]) {
            curr.children[char] = new TrieNode(char)
        }
        curr = curr.children[char]
    }
    
    curr.isEnd = true
};

/**
 * Returns if the word is in the data structure. A word could contain the dot character '.' to represent any one letter. 
 * @param {string} word
 * @return {boolean}
 */
WordDictionary.prototype.search = function(word) {
    return this.find(word, this.root, 0)
};

WordDictionary.prototype.find = function(word, node, index) {
    if (index === word.length)
        return node.isEnd
    
    const char = word[index]
    if (char === ".") {
        for (let key of Object.keys(node.children)) {
            if (this.find(word, node.children[key], index + 1)) {
                return true
            }
        }
        return false
    } else {
        return node.children[char] !== undefined && 
            this.find(word, node.children[char], index + 1)
    }
};

/** 
 * Your WordDictionary object will be instantiated and called as such:
 * var obj = new WordDictionary()
 * obj.addWord(word)
 * var param_2 = obj.search(word)
 */
```

## 720. Longest Word in Dictionary
```javascript
// O(n^2)
var longestWord = function(words) {
    const seen = new Set(words)
    let result = ""
    
    for (let word of words) {
        let prefix = ""
        for (let char of word) {
            let currPrefix = prefix + char
            if (!seen.has(currPrefix)) break
            prefix = currPrefix
        }
        
        if (result.length <= prefix.length) {
            if (result.length === prefix.length && prefix > result) continue
            result = prefix
        }
    }
    
    return result
};

// O(n) Iterative
class TrieNode {
    constructor(val = "") {
        this.val = val
        this.children = {}
        this.isEnd = false
        this.word = ""
    }
}

class Trie {
    root = new TrieNode()

    insert(word) {
        let curr = this.root
        
        for (let char of word) {
            if (!curr.children[char])
                curr.children[char] = new TrieNode(char)
            
            curr = curr.children[char]
        }
        
        curr.isEnd = true
        curr.word = word
    }

    longestWord() {
        let result = ""
        
        const stack = [this.root]
        while (stack.length) {
            const node = stack.pop()

            if (node !== this.root) {
                if (!node.isEnd) continue
                
                if ((result.length < node.word.length) || 
                    (result.length === node.word.length && result > node.word)) {
                    result = node.word
                }
            }
            
            for (const neighbor of Object.values(node.children))
                stack.push(neighbor)
        }
        return result
    }
}

var longestWord = function(words) {
    const trie = new Trie()
    for (let word of words)
        trie.insert(word)
    
    return trie.longestWord()
};

// O(n) Recursive
class TrieNode {
    constructor(val = "") {
        this.val = val
        this.children = {}
        this.isEnd = false
    }
}

class Trie {
    root = new TrieNode()

    insert(word) {
        let curr = this.root
        
        for (let char of word) {
            if (!curr.children[char])
                curr.children[char] = new TrieNode(char)
            
            curr = curr.children[char]
        }
        
        curr.isEnd = true
    }

    longestWord() {
        const _longestWord = (node, str) => {
            if ((result.length < str.length) || (result.length === str.length && str < result))
                result = str
            
            for (let neighbor of Object.values(node.children))
                if (neighbor.isEnd)
                    _longestWord(neighbor, str + neighbor.val)
        }
        
        let result = ""
        _longestWord(this.root, "")
        return result
    }
}

var longestWord = function(words) {
    const trie = new Trie()
    for (let word of words)
        trie.insert(word)
    
    return trie.longestWord()
};
```

## 79. Word Search
```javascript
var exist = function(board, word) {
    for (let row = 0; row < board.length; row++) {
        for (let col = 0; col < board[row].length; col++) {
            if (board[row][col] === word[0] && dfs(board, row, col, 0, word)) {
                return true
            }
        }
    }
    
    return false
};

const dfs = (board, row, col, count, word) => {
    if (count === word.length)
        return true
    
    if (row < 0 || row >= board.length || 
        col < 0 || col >= board[row].length || 
        board[row][col] != word[count]) {
        return false
    }
    
    let temp = board[row][col]
    board[row][col] = ""
    let found = dfs(board, row + 1, col, count + 1, word) || 
                dfs(board, row - 1, col, count + 1, word) || 
                dfs(board, row, col + 1, count + 1, word) ||
                dfs(board, row, col - 1, count + 1, word)
    board[row][col] = temp
    return found
}
```

## 200. Number of Islands
```javascript
// DFS
var numIslands = function(grid) {
    if (!grid.length || !grid[0].length)
        return 0
    
    let numOfIslands = 0
    
    for (let row = 0; row < grid.length; row++) {
        for (let col = 0; col < grid[row].length; col++) {
            if (grid[row][col] === '1') {
                numOfIslands++
                dfs(grid, row, col)
            }
        }
    }
    return numOfIslands
};

const dfs = (grid, row, col) => {
    if (row < 0 || row >= grid.length || col < 0 || col >= grid[0].length || grid[row][col] === '0')
        return
    
    grid[row][col] = '0'
    
    dfs(grid, row + 1, col)
    dfs(grid, row - 1, col)
    dfs(grid, row, col + 1)
    dfs(grid, row, col - 1)  
}

// BFS
var numIslands = function(grid) {
    if (!grid.length || !grid[0].length)
        return 0
    
    let numOfIslands = 0
    const queue = []
    
    for (let row = 0; row < grid.length; row++) {
        for (let col = 0; col < grid[row].length; col++) {
            if (grid[row][col] === '1') {
                numOfIslands++
                
                queue.push([row, col])
                while (queue.length) {
                    const [topRow, topCol] = queue.shift()
                    
                    if (topRow >= 0 && topRow < grid.length && topCol >= 0 && topCol < grid[0].length) {
                        if (grid[topRow][topCol] === '0') continue

                        grid[topRow][topCol] = '0'

                        queue.push([topRow + 1, topCol])
                        queue.push([topRow - 1, topCol])
                        queue.push([topRow, topCol + 1])
                        queue.push([topRow, topCol - 1])
                    }
                }
            }
        }
    }
    return numOfIslands
};
```

## Bucket Sort
```javascript
const bucketSort = arr => {
  const buckets = Array(arr.length).fill(null).map(ele => [])
  const max = Math.max(...arr)
  const divider = Math.ceil((max + 1) / arr.length)
  
  for (const num of arr)
    buckets[Math.floor(num / divider)].push(num)
  
  arr = buckets.flat().sort((a, b) => a - b)
}

const arr = [10, 200, 1000, 32, 444]
bucketSort(arr)
```

## Counting Sort
```javascript
const arr = [1, 3, 2, 3, 5, 5, 6, 3, 7, 8, 9]

const countingSort = arr => {
  const max = Math.max(...arr)
  const counts = Array(max + 1).fill(0)
  for (let num of arr)
    counts[num]++
  
  for (let i = 0; i < counts.length - 1; i++)
    counts[i+1] = counts[i] + counts[i+1]
  
  const result = []
  for (let i = arr.length - 1; i >= 0; i--)
    result[counts[arr[i]]-- - 1] = arr[i]
    
  return result
}

countingSort(arr)
```

## QuickSelect
```javascript
const random = (min, max) => {
  return Math.floor(Math.random() * (max - min + 1)) + min
}

const partition = (arr, start, end) => {
  let pivot = arr[end]
  let i = start - 1
  
  for (let j = start; j < end; j++) {
    if (arr[j] <= pivot) {
      i++
      let temp = arr[i]
      arr[i] = arr[j]
      arr[j] = temp
    }
  }
  
  let temp = arr[i + 1]
  arr[i + 1] = arr[end]
  arr[end] = temp
  
  return i + 1
}

const quickSelect = (arr, k, start, end) => {
  if (start > end) return null
  
  start = start || 0
  end = end || arr.length - 1
  
  let randomIndex = random(start, end)  
  let temp = arr[randomIndex]
  arr[randomIndex] = arr[end]
  arr[end] = temp

  let partitionIndex = partition(arr, start, end)
  if (partitionIndex === arr.length - k - 1) {
    return arr[partitionIndex]
  }
  
  if (partitionIndex > arr.length - k - 1) {
    return quickSelect(arr, k, start, partitionIndex - 1)
  } else {
    return quickSelect(arr, k, partitionIndex + 1, end)
  }
}

const arr = [1, 9, 4, 5, 6, 3, 2, 7, 8]
quickSelect(arr, 2)
```

## Rabin-Karp
```javascript
var rkSearch = function (text, pattern) {
  const tLength = text.length
  const pLength = pattern.length
  
  if (tLength === 0 || pLength === 0 || tLength < pLength) 
    return null
  
  const textArr = text.split('').map(char => char.charCodeAt(0) - "0".charCodeAt(0))
  const patternArr = pattern.split('').map(char => char.charCodeAt(0) - "0".charCodeAt(0))

  const base = 256
  const prime = 101
  
  let textHash = 0
  let patternHash = 0
  
  for (let i = 0; i < pLength; i++) {
    patternHash = (base * patternHash + patternArr[i]) % prime
    textHash = (base * textHash + textArr[i]) % prime
  }
  
  const indices = []
  
  for (let i = 0; i <= tLength - pLength; i++) {
    if (textHash === patternHash) {
      if (text.substring(i, i + pLength) === pattern) {
        indices.push(i)
      }
    }
    
    let h = Math.pow(base, pLength - 1) % prime
    let nextCharCode = textArr[i + pLength]
    textHash = (base * (textHash - textArr[i] * h) + nextCharCode) % prime
    
    if (textHash < 0)
      textHash += prime 
  }
  
  return indices.length ? indices : null
}

rkSearch("aabaaabaaac", "aab")
```

## 1110. Delete Nodes And Return Forest
```javascript
var delNodes = function(root, to_delete) {
    const _delNodes = (node) => {
        if (!node) return null
        
        node.left = _delNodes(node.left)
        node.right = _delNodes(node.right)
        
        if (toDelete.has(node.val)) {
            if (node.left) remaining.push(node.left)
            if (node.right) remaining.push(node.right)
            return null
        }
        
        return node
    }
    
    const toDelete = new Set(to_delete)
    const remaining = []
    
    _delNodes(root)
    
    if (!toDelete.has(root.val))
        remaining.push(root)
    
    return remaining
};
```
## 293. Flip Game
```javascript
var generatePossibleNextMoves = function(s) {
    const result = []
    const str = s.split('')
    
    let i = 1
    while (i < s.length) {
        if (s[i] === '-') {
            i += 2
            continue
        }
        
        if (s[i] === '+' && s[i - 1] === '+') {
            str[i] = '-'
            str[i - 1] = '-'
            result.push(str.join(''))
            
            str[i] = '+'
            str[i - 1] = '+'
        }
        
        i += 1
    }
    
    return result
};
```

## 292. Nim Game
```javascript
var canWinNim = function(n) {
    return n % 4 !== 0
};
```

## 199. Binary Tree Right Side View
```javascript
var rightSideView = function(root) {
    if (!root) return []
    
    const result = []
    const queue = [root]
    
    while (queue.length) {
        const size = queue.length
        
        for (let i = 0; i < size; i++) {
            const curr = queue.shift()
            
            if (i === size - 1)
                result.push(curr.val)
            
            if (curr.left)
                queue.push(curr.left)

            if (curr.right)
                queue.push(curr.right)
        }    
    }
    
    return result
};
```

## 520. Detect Capital
```javascript
var detectCapitalUse = function(word) {
    let capCount = 0
    
    for (let char of word) {
        if (char === char.toUpperCase())
            capCount++
    }

    return capCount === word.length || 
           capCount === 0 || 
           capCount === 1 && word[0] === word[0].toUpperCase()
};
```

## 204. Count Primes
```javascript
var countPrimes = function(n) {
    const primes = Array(n).fill(true)
    primes[0] = false
    primes[1] = false
    
    for (let i = 2; i * i < n; i++) {
        if (primes[i]) {
            for (let j = i; j * i < n; j++) {
                primes[i * j] = false
            }
        }
    }
    
    return primes.reduce((result, ele) => {
        if (!ele) return result
        return result + 1
    }, 0)
};
```

## 680. Valid Palindrome II
```javascript
var validPalindrome = function(s) {
    return isPalindrome(s, (left, right) => {
        return isPalindrome(s, () => false, left + 1, right) || 
               isPalindrome(s, () => false, left, right - 1)       
    })
};

const isPalindrome = (str, callback, i, j) => {
    let left = i || 0
    let right = j || str.length - 1
    
    while (left < right) {
        if (str[left] !== str[right]) {
            return callback(left, right)
        }
        left++
        right--
    }
    return true
}
```

## 383. Ransom Note
```javascript
var canConstruct = function(ransomNote, magazine) {
    const counts = {}
    for (let char of magazine) {
        counts[char] ? counts[char]++ : counts[char] = 1 
    }
    
    for (let char of ransomNote) {
        if (counts[char] <= 0 || counts[char] === undefined) 
            return false
        
        counts[char]--
    }
    
    return true
};
```

## 202. Happy Number
```javascript
var isHappy = function(n) {
    const seen = new Set()
    let curr = n
    
    while (curr !== 1) {
        let sum = 0
        
        while (curr) {
            let digit = curr % 10
            sum += digit * digit
            curr = Math.floor(curr / 10)
        }
        
        if (seen.has(sum)) 
            return false
        
        seen.add(sum)
        curr = sum
    }    
    return true
};
```

## 258. Add Digits
```javascript
var addDigits = function(num) {
    let curr = num
    
    while (Math.floor(curr / 10)) {
        let sum = 0
        
        while (curr) {
            let digit = curr % 10
            sum += digit
            curr = Math.floor(curr / 10)
        }
        curr = sum
    }
    
    return curr
};
```

## 49. Group Anagrams
```javascript
var groupAnagrams = function(strs) {
    const map = {}
    
    for (let str of strs) {
        const charArr = str.split('')
        charArr.sort()
        const sortedStr = charArr.join('')
        
        if (map[sortedStr]) {
            map[sortedStr].push(str)
        } else {
            map[sortedStr] = [str]
        }    
    }
    
    return Object.values(map)
};
```

## 1221. Split a String in Balanced Strings
```javascript
var balancedStringSplit = function(s) {
    let balance = 0
    let count = 0
    
    for (let i = 0; i < s.length; i++) {
        s[i] === 'R' ? balance++ : balance--
        if (balance === 0) count++
    }
    
    return count
};
```

## 90. Subsets II
```javascript
var subsetsWithDup = function(nums) {
    nums.sort((a, b) => a - b)
    const subsets = []
    generateSubsets(0, nums, [], subsets)
    return subsets    
};

const generateSubsets = (index, nums, curr, subsets) => {
    subsets.push(curr.slice())
    
    for (let i = index; i < nums.length; i++) {
        if (i !== index && nums[i] === nums[i - 1]) continue
        curr.push(nums[i])
        generateSubsets(i + 1, nums, curr, subsets)
        curr.pop(nums[i])
    }
}
```

## 78. Subsets
```javascript
var subsets = function(nums) {
    const subsets = []
    generateSubsets(0, nums, [], subsets)
    return subsets
};

const generateSubsets = (index, nums, curr, subsets) => {
    subsets.push(curr.slice())
    
    for (let i = index; i < nums.length; i++) {
        curr.push(nums[i])
        generateSubsets(i + 1, nums, curr, subsets)
        curr.pop(nums[i])
    }
}
```

## 11. Container With Most Water
```javascript
var maxArea = function(height) {
    let maxArea = 0
    let left = 0
    let right = height.length - 1
    
    while (left < right) {
        let currWidth = right - left
        let currHeight = Math.min(height[left], height[right])
        let area = currWidth * currHeight
        maxArea = Math.max(maxArea, area)
        
        if (height[left] >= height[right]) {
            right--
        } else {
            left++
        }
    }
    
    return maxArea
};
```

## 881. Boats to Save People
```javascript
var numRescueBoats = function(people, limit) {
    people.sort((a, b) => a - b)
    
    let boats = 0
    let left = 0
    let right = people.length - 1
    
    while (left <= right) {
        let weight = people[left] + people[right]
        
        if (weight > limit) {
            right--
        } else {
            left++
            right--
        }
        boats++
    }
    
    return boats
};
```

## 875. Koko Eating Bananas
```javascript
var minEatingSpeed = function(piles, H) {
    let low = 0
    let high = 1000000000
    
    while (low < high) {
        let mid = Math.floor((high - low) / 2) + low
        if (!possible(piles, H, mid)) {
            low = mid + 1
        } else {
            high = mid
        }
    }
    
    return low
};

const possible = (piles, H, K) => {
    let time = 0
    for (const pile of piles)
        time += Math.ceil(pile / K)
    
    return time <= H
}
```

## 113. Path Sum II
```javascript
var pathSum = function(root, sum) {
    const _pathSum = (node, currSum, path) => {
        if (!node) return
        
        currSum += node.val
        path.push(node.val)
        
        if (currSum === sum && !node.left && !node.right) {
            result.push(path.slice())
        } else {
            _pathSum(node.left, currSum, path)
            _pathSum(node.right, currSum, path)    
        }
        
        path.pop()
    }
    
    const result = []
    _pathSum(root, 0, [])
    return result
};
```

## 482. License Key Formatting
```javascript
var licenseKeyFormatting = function(S, K) {
    const arr = S.split('').filter(char => char !== '-')
    const remaining = arr.length % K
    const result = []    
    let charArr = []
    
    if (remaining) {    
        for (let i = 0; i < remaining; i++)
            charArr.push(arr[i].toUpperCase())
        
        result.push(charArr.join(''))
        charArr = []
    }
    
    for (let i = remaining; i < arr.length; i++) {
        charArr.push(arr[i].toUpperCase())
        
        if (charArr.length === K) {
            result.push(charArr.join(''))
            charArr = []
        }
    }

    return result.join('-')  
};
```

## 415. Add Strings
```javascript
var addStrings = function(num1, num2) {
    if (!num1) return num2
    if (!num2) return num1
    
    let i = num1.length - 1
    let j = num2.length - 1
    let carry = 0
    
    let result = []
    
    while (i >= 0 || j >= 0 || carry) {
        let num1Val = i >= 0 ? +num1[i] : 0
        let num2Val = j >= 0 ? +num2[j] : 0
        
        let sum = num1Val + num2Val + carry
        carry = Math.floor(sum / 10)
        result.push(sum % 10)
        i--
        j--
    }
    
    return result.reverse().join('')
};
```

## 508. Most Frequent Subtree Sum
```javascript
var findFrequentTreeSum = function(root) {
    const dfs = (node) => {
        if (!node) return 0
        
        const sum = node.val + dfs(node.left) + dfs(node.right)
        
        sumFreq[sum] ? sumFreq[sum]++ : sumFreq[sum] = 1
        maxFreq = Math.max(sumFreq[sum], maxFreq)
        return sum
    }
    
    const sumFreq = {}
    let maxFreq = 0
    dfs(root)
    
    return Object.entries(sumFreq).reduce((result, [key, val]) => {
        if (val === maxFreq)
            result.push(key)
        return result
    }, [])
};
```

## 366. Find Leaves of Binary Tree
```javascript
var findLeaves = function(root) {
    const dfs = (node) => {
        if (!node) return -1
        
        const leftDepth = dfs(node.left)
        const rightDepth = dfs(node.right)
        const depth = Math.max(leftDepth, rightDepth) + 1
        depths[depth] ? depths[depth].push(node.val) : depths[depth] = [node.val]
        return depth
    }
    
    const depths = []
    dfs(root)
    return depths
};
```

## 695. Max Area of Island
```javascript
var maxAreaOfIsland = function(grid) {
    let max = 0
    
    for (let row = 0; row < grid.length; row++) {
        for (let col = 0; col < grid[0].length; col++) {
            if (grid[row][col] !== 1) continue
            max = Math.max(max, area(grid, row, col))
        }
    }
    
    return max
};

const area = (grid, row, col) => {
    const _area = (grid, row, col) => {
        if (row < 0 || row >= grid.length ||
            col < 0 || col >= grid[0].length || 
            grid[row][col] === 0) 
            return
        
        grid[row][col] = 0
        area++
        
        _area(grid, row - 1, col)
        _area(grid, row + 1, col)
        _area(grid, row, col - 1)
        _area(grid, row, col + 1)
    }
    
    let area = 0
    _area(grid, row, col)
    return area
}
```

## 463. Island Perimeter
```javascript
var islandPerimeter = function(grid) {
    for (let row = 0; row < grid.length; row++) {
        for (let col = 0; col < grid[0].length; col++) {
            if (grid[row][col] === 1) {
                return perimeter(grid, row, col)   
            }
        }
    }
    return 0
};

const perimeter = (grid, row, col) => {
    const _perimeter = (grid, row, col) => {
        if (row < 0 || row >= grid.length || 
            col < 0 || col >= grid[0].length || 
            grid[row][col] !== 1) return
        
        grid[row][col] = 'X'
        
        if (row - 1 < 0 || !grid[row - 1][col]) p++
        if (row + 1 >= grid.length || !grid[row + 1][col]) p++
        if (col - 1 < 0 || !grid[row][col - 1]) p++
        if (col + 1 >= grid[0].length || !grid[row][col + 1]) p++

        _perimeter(grid, row - 1, col)
        _perimeter(grid, row + 1, col)
        _perimeter(grid, row, col - 1)
        _perimeter(grid, row, col + 1)
    }
    
    let p = 0
    _perimeter(grid, row, col)
    return p
}
```

## 994. Rotting Oranges
```javascript
var orangesRotting = function(grid) {
    let maxTime = 0
    const queue = []
    
    for (let row = 0; row < grid.length; row++)
        for (let col = 0; col < grid[0].length; col++)
            if (grid[row][col] === 2)
                queue.push([row, col, 0])
    
    while (queue.length) {
        const [row, col, time] = queue.shift()
        
        if (row < 0 || row >= grid.length || 
            col < 0 || col >= grid[0].length || 
            grid[row][col] === 0) continue
        
        grid[row][col] = 0
        maxTime = Math.max(maxTime, time)
        
        queue.push([row - 1, col, time + 1])
        queue.push([row + 1, col, time + 1])
        queue.push([row, col + 1, time + 1])
        queue.push([row, col - 1, time + 1])
    }
    
    for (let row = 0; row < grid.length; row++)
        for (let col = 0; col < grid[0].length; col++)
            if (grid[row][col] === 1)
                return -1
    
    return maxTime  
};
```