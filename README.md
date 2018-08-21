# swift-algorithms



## Bubble Sort
Best - O(n)
Worst - O(n^2)
Space - O(1)

```swift
extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  mutating func bubbleSort() {
    guard count > 1 else { return }
    
    for endingIndex in indices.reversed() {
      var noSwaps = true
      for currentIndex in startIndex..<endingIndex {
        let nextIndex = index(after: currentIndex)
        if self[currentIndex] > self[nextIndex] {
          swapAt(currentIndex, nextIndex)
          noSwaps = false
        }
      }
      if noSwaps { return }
    }
  }
}

var a = [6, 3, 8, 7, 5, 4, 2, 9, 8, 5, 4, 2]
a.bubbleSort()
```


## Insertion Sort
Best - O(n)
Worst - O(n^2)
Space - O(1)

```swift
extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  mutating func insertionSort() {
    guard count > 1 else { return }
    
    for currentIndex in index(after: startIndex)..<endIndex {
      for shiftingIndex in (index(after: startIndex)...currentIndex).reversed() {
        let previousIndex = index(before: shiftingIndex)
        if self[previousIndex] > self[shiftingIndex] {
          swapAt(previousIndex, shiftingIndex)
        } else {
          break
        }
      }
    }
  }
}

var b = [6, 3, 8, 7, 5, 4, 2, 9, 8, 5, 4, 2]
b.insertionSort()
```

## Selection Sort
Best - O(n^2)
Worst - O(n^2)
Space - O(1)

```swift
extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  mutating func selectionSort() {
    guard count > 1 else { return }
    
    for currentIndex in startIndex..<index(before: endIndex) {
      var lowestIndex = currentIndex
      for swapIndex in index(after: currentIndex)..<endIndex {
        if self[swapIndex] < self[lowestIndex] {
          lowestIndex = swapIndex
        }
      }
      swapAt(lowestIndex, currentIndex)
    }
  }
}

var c = [6, 3, 8, 7, 5, 4, 2, 9, 8, 5, 4, 2]
c.selectionSort()
```

## Merge Sort
Best - O(n log n)
Worst - O(n log n)
Space - O(n log n)

```swift
public func mergeSort<Element: Comparable>(_ array: [Element]) -> [Element] {
  guard array.count > 1 else { return array }
  let middleIndex = array.count / 2
  let left = mergeSort(Array(array[..<middleIndex]))
  let right = mergeSort(Array(array[middleIndex...]))
  return merge(left, right)
}

private func merge<Element: Comparable>(_ left: [Element], _ right: [Element]) -> [Element] {
  var result = [Element]()
  var leftIndex = 0
  var rightIndex = 0
  
  while left.count > leftIndex && right.count > rightIndex {
    let leftElement = left[leftIndex]
    let rightElement = right[rightIndex]
    
    if leftElement > rightElement {
      result.append(rightElement)
      rightIndex += 1
    } else if leftElement < rightElement {
      result.append(leftElement)
      leftIndex += 1
    } else {
      result.append(leftElement)
      result.append(rightElement)
      leftIndex += 1
      rightIndex += 1
    }
  }
  
  if left.count > leftIndex {
    result.append(contentsOf: left[leftIndex...])
  }
  
  if right.count > rightIndex {
    result.append(contentsOf: right[rightIndex...])
  }
  
  return result
}

var d = [6, 3, 8, 7, 5, 4, 2, 9, 8, 5, 4, 2]
mergeSort(d)
```

## Radix Sort
Best - O(nk)
Worst - O(nk)
Space - O(n + k)

```swift
extension Array where Element == Int {
  mutating func radixSort() {
    guard count > 1 else { return }
    
    var done = false
    var digits = 1
    let radix = 10
    
    while !done {
      done = true
      var buckets: [[Int]] = .init(repeating: [], count: radix)
      
      for number in self {
        let remainingPart = number / digits
        let digit = remainingPart % radix
        buckets[digit].append(number)
        
        if remainingPart > 0 {
          done = false
        }
      }
      
      self = buckets.flatMap { $0 }
      digits *= 10
    }
  }
}

var a = [1, 4, 3, 5, 6, 7, 8, 5, 4, 3, 2, 8, 7]
a.radixSort()
```

## Quick Sort
Best - O(n log n)
Worst - O(n^2)
Space - O(1)

```swift
import Foundation

public func quickSort<Element: Comparable>(_ array: inout [Element]) {
  quickSort(&array, array.startIndex, array.index(before: array.endIndex))
}

private func quickSort<Element: Comparable>(_ array: inout [Element], _ low: Int, _ high: Int) {
  guard low < high else { return }
  
  let randomIndex = random(low, high)
  array.swapAt(randomIndex, high)
  
  let partitionIndex = partition(&array, low, high)
  quickSort(&array, low, array.index(before: partitionIndex))
  quickSort(&array, array.index(after: partitionIndex), high)
}

private func partition<Element: Comparable>(_ array: inout [Element], _ low: Int, _ high: Int) -> Int {
  let partitionElement = array[high]
  var lowIndex = low
  
  for currentIndex in low..<high {
    if array[currentIndex] <= partitionElement {
      array.swapAt(currentIndex, lowIndex)
      lowIndex += 1
    }
  }
  
  array.swapAt(lowIndex, high)
  return lowIndex
}

private func random(_ min: Int, _ max: Int) -> Int {
  return min + Int(arc4random_uniform(UInt32(max - min) + 1))
}

var a = [5, 3, 2, 7, 6, 8, 7, 3, 2, 1, 3, 4, 6, 8]
quickSort(&a)
```

## Binary Search (Recursive)
Best - O(log n)
Worst - O(log n)
Space - O(log n)

```swift

```

## Binary Search (Iterative)
Best - O(log n)
Worst - O(log n)
Space - O(1)

```swift

```

## Two Sum (Sorted Array Guaranteed)

```swift
func twoSum(_ array: [Int], _ sum: Int) -> Bool {
  var lowIndex = 0
  var highIndex = array.count - 1
  
  while lowIndex < highIndex {
    let sumOfItems = array[lowIndex] + array[highIndex]
    if sumOfItems == sum {
      return true
    } else if sumOfItems > sum {
      highIndex -= 1
    } else if sumOfItems < sum {
      lowIndex += 1
    }
  }
  return false
}
```


## Two Sum (Sorted Array Not Guaranteed)

```swift
func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
  var dict = [Int: Int]()
  
  for (index, num) in nums.enumerated() {
    let complement = target - num
    
    if let complementIndex = dict[complement] {
      return [complementIndex, index]
    }
    
    dict[num] = index
  }
  return []
}
```

## Two Sum - Input is a Binary Search Tree


## FizzBuzz
```swift
struct FizzBuzz: Sequence, IteratorProtocol {
  var count = 1
  
  mutating func next() -> String? {
    defer { count += 1 }
    return value(for: count)
  }
  
  private func value(for number: Int) -> String {
    if number.divisible(by: 15) { return "FizzBuzz" }
    if number.divisible(by: 3) { return "Fizz" }
    if number.divisible(by: 5) { return "Buzz" }
    return "\(number)"
  }
}

extension Int {
  func divisible(by denominator: Int) -> Bool {
    return self % denominator == 0
  }
}

for value in FizzBuzz().prefix(15) {
  print(value)
}

```


## Fibonacci
```swift
struct Fibonacci: Sequence, IteratorProtocol {
  var state = (0, 1)
  
  mutating func next() -> Int? {
    let upcomingNumber = state.0
    state = (state.1, state.0 + state.1)
    return upcomingNumber
  }
}


for num in Fibonacci().prefix(10) {
  print(num)
}
```

## Most Common Element in an array

```swift
var a = [1, 1, 1, 2, 3, 4, 5, 5, 6, 7, 8, 9, 10, 2, 3, 4, 1, 2, 3, 3, 3, 4, 5]


func mostFrequent<T: Hashable>(array: [T]) -> (value: T, count: Int)? {
  
  let counts = array.reduce(into: [:]) { $0[$1, default: 0] += 1 }
  
  if let (value, count) = counts.max(by: { $0.1 < $1.1 }) {
    return (value, count)
  }
  
  // array was empty
  return nil
}

if let result = mostFrequent(array: a) {
  print("\(result.value) occurs \(result.count) times")
}


```


## Shuffle Array in place