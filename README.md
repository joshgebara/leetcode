# InterviewPrep

## Fib
### Recursive
```javascript
// Time - O(2^n)
// Space - O(n)
const fib = (n) => {
  if (n <= 2) {
    return 1
  } else {
    return fib(n - 1) + fib(n - 2)
  }
}
fib(6)
```

### Iterative
```javascript
// Time - O(n)
// Space - O(1)
const fib2 = (n) => {
  var state = [0, 1]
  for (let x = 0; x < n; x++) {
    state = [state[1], state[0] + state[1]]
  }
  return state[0]
}

fib2(6)
```

## Factorial
### Recursive
```javascript
// Time - O(n)
// Space - O(n)
const factorial = (n) => {
  if (n <= 2) return n
  return n * factorial(n - 1)
}

factorial(10)
```

## Bubble Sort
```javascript
// Time - O(n^2)
// Space - O(1)
const bubbleSort = (numbers, areInIncreasingOrder = (a, b) => { return a > b }) => {
  var noSwaps = true
  for (let endIndex = numbers.length; endIndex > 0; endIndex--) {
    for (let currentIndex = 0; currentIndex < endIndex; currentIndex++) {
      const nextIndex = currentIndex + 1
      if (areInIncreasingOrder(numbers[currentIndex], numbers[nextIndex])) {
        const temp = numbers[nextIndex]
        numbers[nextIndex] = numbers[currentIndex]
        numbers[currentIndex] = temp
        noSwaps = false
      }
    }
    if (noSwaps) return numbers
  }
  return numbers
}

bubbleSort([4, 6, 5, 7], (a, b) => { return a < b });
```

## Insertion Sort
```javascript
// Time - O(n^2)
// Space - O(1)
const insertionSort = (numbers) => {
  for (let currentIndex = 0; currentIndex < numbers.length; currentIndex++) {
    for (let swapIndex = currentIndex + 1; swapIndex > 0; swapIndex--) {
      let previousIndex = swapIndex - 1
      if (numbers[previousIndex] > numbers[swapIndex]) {
        var temp = numbers[previousIndex]
        numbers[previousIndex] = numbers[swapIndex]
        numbers[swapIndex] = temp
      } else {
        break
      }
    } 
  }
  
  return numbers
}

insertionSort([6, 4, 5, 4, 3, 8, 7, 9, 8, 7, 2, 3, 2, 1])  
```