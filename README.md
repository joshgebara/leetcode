# swift-algorithms


traverse a matrix
reverse a utf8 string

http://omarrr.com/underscores-in-apples-swift-numbers/
https://www.raywenderlich.com/1220-swift-tutorial-initialization-in-depth-part-1-2



### Algorithms:

```swift

let unsolvedAlgorithms = [
  "Valid Parentheses",
  "Lowest Common Ancestor of a Binary Search Tree",
  "Reverse Linked List",
  "Best Time to Buy and Sell Stock",
  "Merge Sorted Array",
  "Count and Say",
  "Palindrome Linked List",
  "Valid Palindrome",
  "Add Binary",
  "Hamming Distance",
  "Sqrt(x)",
  "Missing Number",
  "Implement strStr()",
  "Excel Sheet Column Title"
]

let solvedAlgorithms = [
  "Add Two Numbers",
  "Binary Search - Iterative",
  "Binary Search - Recursive",
  "Bubble Sort",
  "Fibonnaci - Iterative",
  "Fibonnaci - Recursive",
  "Find of perfect squares between two numbers",
  "FizzBuzz",
  "Insertion Sort",
  "Jewels and Stones",
  "Merge Sort",
  "Move Zeroes",
  "Quick Sort",
  "Radix Sort",
  "Remove Duplicates from Sorted Array",
  "Roman to Integer",
  "Selection Sort",
  "Shuffle Array",
  "Two Sum - Sorted",
  "Two Sum - Unsorted"
]

import Darwin

extension Collection where Index == Int {
  func randomElements(to number: Int) -> [Element] {
    var result: [Element] = []
    var index: [Int: Int] = [:]
    while result.count < number {
      let randomIndex = Int(arc4random_uniform(UInt32(endIndex)))
      if index[randomIndex] == nil {
        let randomElement = self[randomIndex]
        index[randomIndex] = randomIndex
        result.append(randomElement)
      }
    }
    return result
  }
}

extension MutableCollection where Index == Int {
  mutating func shuffle() {
    guard count > 1 else { return }
    
    for index in indices {
      let randomIndex = Int(arc4random_uniform(UInt32(endIndex)))
      swapAt(index, randomIndex)
    }
  }
}

func problems() -> [String] {
  var algorithms = solvedAlgorithms.randomElements(to: 6)
  algorithms.shuffle()
  algorithms.append(unsolvedAlgorithms[0])
  return algorithms
}

print(problems())

```

## number of jewels
```swift

class Solution {
  func numJewelsInStones(_ J: String, _ S: String) -> Int {
    let jewels = S.filter { J.contains($0) }
    return jewels.count
  }
}



//why is unicode scalar solution faster?

class Solution {
    func numJewelsInStones(_ J: String, _ S: String) -> Int {
        return S.unicodeScalars.filter { J.unicodeScalars.contains($0) }.count
    }
}

extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    var count = 0
    for element in self {
      if predicate(element) {
        count += 1
      }
    }
    return count
  }
}

class Solution {
  func numJewelsInStones<T: Sequence>(_ J: T, _ S: T) -> Int where T.Iterator.Element: Equatable  {
    return S.count(where: { J.contains($0) })
  }
}

extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return filter { predicate($0) }.count
  }
}

class Solution {
  func numJewelsInStones<T: Sequence>(_ J: T, _ S: T) -> Int where T.Iterator.Element: Equatable  {
    return S.count(where: { J.contains($0) })
  }
}

Solution().numJewelsInStones("aA", "aAAbbbb")


extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return filter { predicate($0) }.count
  }
}

class Solution {
  func numJewelsInStones<T: Sequence>(_ J: T, _ S: T) -> Int where T.Iterator.Element: Equatable  {
    return S.count(where: J.contains)
  }
}

Solution().numJewelsInStones("aA", "aAAbbbb")


Time Complexity: O(J\text{.length} + S\text{.length}))O(J.length+S.length)). The O(J\text{.length})O(J.length) part comes from creating J. The O(S\text{.length})O(S.length) part comes from searching S.

Space Complexity: O(J\text{.length})O(J.length).



extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return filter { predicate($0) }.count
  }
}

class Solution {
  func numJewelsInStones(_ J: String, _ S: String) -> Int {
    let jSet = Set(J)
    return S.count(where: jSet.contains)
  }
}

// using a set will be faster because the init is O(n) but subseqent contains calls will be O(1)
// where using an array, each contains call will be O(n)


```



two sum sorted -- two pointer approach 
time O(n)
space - O(1) because it's always two pointers

two sum unsorted -- hash approach
time O(n)
space - O(n) becasue of the hash table

if we had an unsorted array and wanted to optimize for space and not speed then we would do an inplace sort then we would do the two pointer approach.





while a true inplace sort is not possible in swift, making a new array could be a quick operation
especially if we set reservecapacity so it didn't have to keep making new arrays assuming we know the input size. We don't need to rely on geometric growth here.




## roman to int 
```swift 
class Solution {
  func romanToInt(_ s: String) -> Int {
    var accumulator = 0

    for index in 0..<s.count {
      var romanValue = value(for: s[index])

      let nextIndex = index + 1
      if nextIndex < s.count, value(for: s[nextIndex]) > value(for: s[index]) {
        romanValue *= -1
      }
      accumulator += romanValue
    }
    return accumulator
  }

  private static let romanValues: [Character: Int] = ["I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000]

  private func value(for character: Character) -> Int {
    return Solution.romanValues[character] ?? 0
  }
}

extension String {
  subscript (offset: Int) -> Character {
    return self[index(startIndex, offsetBy: offset)]
  }
}

Solution().romanToInt("XXVIIIXDDM")

// Switch statement is faster than a dictionary lookup
// Switch is faster because of wrapping and unwrapping values from the dictionary slows it down
```

# Completed

https://www.youtube.com/watch?v=p3vVYNngyxs


https://www.youtube.com/watch?v=pDyh9VOMWgI




https://stackoverflow.com/questions/22666535/shared-ancestor-between-two-views


https://medium.com/journey-of-one-thousand-apps/finding-the-first-common-superview-in-swift-4abac8a87d84
https://medium.com/journey-of-one-thousand-apps/data-structures-35b968bcedf5
https://medium.com/journey-of-one-thousand-apps/building-with-python-requests-d9260b26e7ab

https://medium.com/journey-of-one-thousand-apps/building-a-hash-data-structure-in-swift-e9b2733d9e20

https://medium.com/journey-of-one-thousand-apps/data-structures-in-the-real-world-508f5968545a

https://medium.com/journey-of-one-thousand-apps/background-thread-and-main-dispatch-50f19b000d8
https://medium.com/journey-of-one-thousand-apps/testing-asynchronous-code-in-swift-2142901dbd4f
https://medium.com/journey-of-one-thousand-apps/complexity-and-big-o-notation-in-swift-478a67ba20e7
https://stackoverflow.com/questions/43230994/common-parent-view-in-swift




https://www.youtube.com/user/tusharroy2525/videos




## Implement strStr()

Knuth–Morris–Pratt(KMP) Pattern Matching(Substring search)
https://www.youtube.com/watch?v=GTJr8OvyEVQ
// O(m + n)
m = length of haystack
n = length of needle






https://www.youtube.com/watch?v=WIoZuhAC1p4


##two sum

extension Collection where Element == Int {
  func twoSum(for target: Element) -> (Element, Element)? {
    guard count > 1 else { return nil }
    
    var elementsSeen = Set<Int>()
    elementsSeen.reserveCapacity(Int(count))
    
    for element in self {
      print(elementsSeen)
      let compliment = target - element
      if elementsSeen.contains(compliment) {
        return (compliment, element)
      } else {
        elementsSeen.insert(element)
      }
    }
    
    return nil
  }
}

var a = [1, 5, 4, 8, 66, 4, 12, 3, 2, 1, 6, 7, 8, 99, 0]
print(a.twoSum(for: 100))

we can reserve capacity because we don't need a geometric growth strategy. we already know the maximum amount of numbers possible is equal to the count of the collection so we can just reserve capacity for the size of the collection




## Add Two Numbers
// Time complexity : O(\max(m, n))O(max(m,n)). Assume that mm and nn represents the length of l1l1 and l2l2 respectively, the algorithm above iterates at most \max(m, n)max(m,n) times.
// Space complexity : O(\max(m, n))O(max(m,n)). The length of the new list is at most \max(m,n) + 1max(m,n)+1.
```swift
func addTwoNumbers(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
  var currentL1Node = l1
  var currentL2Node = l2
  
  let head = ListNode(0)
  var current = head
  var carry = 0
  
  while currentL1Node != nil || currentL2Node != nil {
    let l1NodeValue = currentL1Node?.val ?? 0
    let l2NodeValue = currentL2Node?.val ?? 0

    let total = l1NodeValue + l2NodeValue + carry
    carry = total / 10
    current.next = ListNode(total % 10)
    current = current.next!
    currentL1Node = currentL1Node?.next
    currentL2Node = currentL2Node?.next
  }
  
  if carry > 0 {
    current.next = ListNode(carry)
  }
  return head.next
}
```


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

extension RandomAccessCollection where Element == Int {
  func twoSum(for sum: Int) -> (Int, Int)? {
    guard count > 1 else { return nil }
    
    var left = startIndex
    var right = index(before: endIndex)
    
    while left < right {
      let value = self[left] + self[right]
      
      if value == sum {
        return (self[left], self[right])
      } else if value > sum {
        right = index(before: right)
      } else {
        left = index(after: left)
      }
    }
    return nil
  }
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

extension RandomAccessCollection where Element == Int {
  func twoSum(for target: Int) -> (Int, Int)? {
    guard count > 1 else { return nil }
    var numbersSeen = Set<Int>()
    
    for num in self {
      let complement = target - num
      if numbersSeen.contains(complement) {
        return (complement, num)
      } else {
        numbersSeen.insert(num)
      }
    }
    
    return nil
  }
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









var a = [1, 2, 3, 4]
withUnsafePointer(to: &a) { print("\($0)") }

var b = [1...1000]
withUnsafePointer(to: &b) { print("\($0)") }
var c = [1...1000]
var d = [1...1000]

a.append(contentsOf: 5...500)
withUnsafePointer(to: &b) { print("\($0)") }
a.capacity
withUnsafePointer(to: &a) { print("\($0)") }

https://en.wikipedia.org/wiki/In-place_algorithm#In_functional_programming


### jewels in stones
```swift 
infix operator ===

extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    var count = 0
    
    for element in self {
      if predicate(element) {
        count += 1
      }
    }
    
    return count
  }
}

extension Sequence where Element: Hashable {
  func occurrences(in other: Self) -> Int {
    let elementSet = Set(self)
    return other.count(where: elementSet.contains)
  }
  
  static func === (lhs: Self, rhs: Self) -> Int {
    return lhs.occurrences(in: rhs)
  }
}

"Aa" === "AAAbbbbbaaaaBBB"
```




struct Alphabet: Collection {
    var startIndex = 0
    var endIndex = 26
    
    private let base = 65
    
    func index(after index: Int) -> Int {
        guard index < endIndex else { fatalError() }
        return index + 1
    }
    
    subscript(offset: Int) -> String {
        guard offset < endIndex,
              let unicode = UnicodeScalar(base + offset) else { fatalError() }
        return String(describing: unicode)
    }
}


let alphabet = Alphabet()
alphabet[25]





postfix operator ~


struct FizzBuzz: Sequence, IteratorProtocol {
    var current = 1
    
    mutating func next() -> String? {
        defer { current += 1 }
        return FizzBuzz.value(for: current)
    }
    
    static func value(for number: Int) -> String {
        if number.divisible(by: 15) { return "FizzBuzz" }
        if number.divisible(by: 3)  { return "Fizz" }
        if number.divisible(by: 5)  { return "Buzz" }
        return "\(number)"
    }
}

extension Int {
    func divisible(by number: Int) -> Bool {
        return self % number == 0
    }
    
    static postfix func ~ (number: Int) -> String {
        return FizzBuzz.value(for: number)
    }
}

for number in 1..<100 {
    print(number~)
}



extension BidirectionalCollection where Element == Int {
    func twoSum(for target: Element) -> (Element, Element)? {
        guard count > 1 else { return nil }
        
        var left = startIndex
        var right = index(before: endIndex)
        
        while left <= right {
            let sum = self[left] + self[right]
            
            if sum == target {
                return (self[left], self[right])
            }
            
            if sum > target {
                right = index(before: right)
            }
            
            if sum < target {
                left = index(after: left)
            }
        }
        return nil
    }
}

var a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
a.twoSum(for: 13)



infix operator ~~

extension Sequence {
    func count(where predicate: (Element) -> Bool) -> Int {
        var count = 0
        for element in self where predicate(element)  {
            count += 1
        }
        return count
    }
}

extension Sequence where Element: Hashable {

    static func ~~ (_ lhs: Self, rhs: Self) -> Int {
        return lhs.occurrences(in: rhs)
    }

    func occurrences(in other: Self) -> Int {
        let elementSet = Set(self)
        return other.count(where: elementSet.contains)
    }
}

class Solution {
    func numJewelsInStones(_ J: String, _ S: String) -> Int {
        return J.unicodeScalars ~~ S.unicodeScalars
    }
}

"Aa" ~~ "AaaaaBBBBCCaaaa"








class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        guard nums.count > 1 else { return nums.count }
        
        var i = 0
        
        for j in 1..<nums.endIndex {
            if nums[i] != nums[j] {
                i += 1
                nums[i] = nums[j]
            }
        }
        
        return i + 1
    }
}




    func moveZeroes(_ nums: inout [Int]) {
        var currentIndex = 0
        var previousIndex = 0

        while currentIndex < nums.count {
            if nums[currentIndex] != 0 {
                nums.swapAt(previousIndex, currentIndex)
                previousIndex += 1
            }
            currentIndex += 1
        }
    }