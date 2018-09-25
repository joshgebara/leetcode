# swift-algorithms


traverse a matrix
reverse a utf8 string

http://omarrr.com/underscores-in-apples-swift-numbers/
https://www.raywenderlich.com/1220-swift-tutorial-initialization-in-depth-part-1-2


https://stackoverflow.com/questions/3255/big-o-how-do-you-calculate-approximate-it

https://www.khanacademy.org/computing/computer-science/algorithms/intro-to-algorithms/v/what-are-algorithms

https://www.youtube.com/watch?v=y2b94AxPlF8

### Algorithms:

```swift


let unsolvedAlgorithms = [
  "N-Queens",
  "Hamming Distance",
  "Challenge 4: Does one string contain another? - KMP algo",
  "Missing Number",
  "Best Time to Buy and Sell Stock",
  "Count and Say",
  "Add Binary",
  "Lowest Common Ancestor of a Binary Search Tree - https://www.youtube.com/watch?v=GnliEfQo114",
  "Valid Parentheses",
  "Traverse a matrix",
  "Tic tac toe",
  "Reverse a utf 8 string",
  "Merge Sorted Array",
  "Implement strStr()",
  "Sqrt(x)",
  "Heap Sort",
  "Rotate String - (Leetcode) - Knuth-Morris-Pratt",
  "Excel Sheet Column Title",
  "Towers of Hanoi",
  "Valid Palindrome - (Leetcode)",
  "Merge Sort Linked List",
  "Merge Sort - Iterative",
  "Quick Sort - Iterative",
  "Permutations",
  "Sort a linked list"
]

let unimplementedDataStructures = [
    "Stack",
  "Queue",
  "Tree",
  "Binary Tree",
  "Binary Search Tree",
  "Trie",
  "Max Heap",
  "Min Heap",
  "Graph"
]

let implementedDataStructures = [
  "LinkedList"
]

let solvedAlgorithms = [
  "Binary Search - Iterative",
  "Binary Search - Recursive",
  "Bubble Sort",
  "Fibonnaci - Iterative",
  "Fibonnaci - Recursive",
  "Find of perfect squares between two numbers",
  "FizzBuzz",
  "Insertion Sort",
  "Jewels and Stones - (Leetcode)",
  "Merge Sort",
  "Move Zeroes - (Leetcode)",
  "Quick Sort",
  "Radix Sort",
  "Remove Duplicates from Sorted Array - (Leetcode)",
  "Roman to Integer - (Leetcode)",
  "Selection Sort",
  "Shuffle Array - (Austin/Facebook)",
  "Two Sum - Sorted - (Leetcode)",
  "Two Sum - Unsorted - (Leetcode)",
  "Reverse Linked List - Iterative",
  "Reverse Linked List - Recursive",
  "Maximum Subarray - Kadane's Algorithm",
  "Pow(x, n)",
  "Factorial",
  "Create a function that prints out the elements of a linked list in reverse order",
  "Create a function that returns the middle node of a linked list",
  "Create a function that reverses a linked list",
  "Create a function that takes two sorted linked lists and merges them into a single sorted linked list",
  "Delete node in a linked list",
  "Remove linked list elements",
  "Intersection of two linked lists",
  "Linked list cycle",
  "Palindrome linked list",
  "Remove Nth Node From End of List",
  "Add Two Numbers - (Leetcode) this is a linked list problem",
  "Challenge 1: Are the letters unique?",
  "Challenge 2: Is a string a palindrome?",
  "Challenge 3: Do two strings contain the same characters?",
  "Challenge 5: Count the characters",
  "Challenge 6: Remove duplicate letters from a string"
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

func problems() {
  print("-----Algorithms to Practice-----")
  let algorithms = solvedAlgorithms.randomElements(to: 8)
  algorithms.forEach { print($0) }
  print("\n")
  
  print("-----Data Structure to Implement-----")
  print(implementedDataStructures.randomElements(to: 1).first!)
  print("\n")
  
  print("-----Algorithm to Learn-----")
  print(unsolvedAlgorithms[0])
  print("\n")
}

problems()


```


extension Int {
  static func random(in range: Range<Int>) -> Int {
    let offset = abs(range.lowerBound)
    let min = UInt32(range.lowerBound + offset)
    let max = UInt32(range.upperBound + offset)
    return Int(min + arc4random_uniform(max - min)) - offset
  }
}


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
// switches can also be faster based on how the compile treats it. For example, switches can be made with jump tables or even binary searchs or a long series of if statements. There are no gauratees. Switches can be faster than dictionaries, but dictionaries are always gaurantees a constant time lookup and they are easier to maintain therefore are the preferred choice
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
infix operator ~

extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return filter(predicate).count
  }
}

extension Sequence where Element: Hashable {
  static func ~ (lhs: Self, rhs: Self) -> Int {
    return rhs.occurences(in: lhs)
  }
  
  func occurences(in other: Self) -> Int {
    let elementSet = Set(self)
    return other.count(where: elementSet.contains)
  }
}

class Solution {
  func numJewelsInStones(_ J: String, _ S: String) -> Int {
    return S ~ J
  }
}
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





    extension Sequence {
  func map<T>(while predicate: (Element) -> Bool, with transform: (Element) -> T) -> [T] {
    var result = [T]()
    
    for element in self where predicate(element) {
      result.append(transform(element))
    }
    
    return result
  }
}

func perfectSquares(in range: CountableRange<Int>) -> [Int] {
  guard range.count > 0 else { return [] }
  
  return range.map(while: { $0 * $0 < range.upperBound }, with: { $0 * $0 })
}

perfectSquares(in: 1..<20)


// Find of perfect squares between two numbers

extension CountableRange where Bound == Int {
  func perfectSquares() -> [Int] {
    guard lowerBound < upperBound else { return [] }
    
    var squareRoot = lowerBound
    
    return reduce(into: [], { (result, element) in
      let square = squareRoot * squareRoot
      guard square < upperBound else { return }
      result.append(square)
      squareRoot += 1
    })
  }
}




extension RandomAccessCollection where Element: Comparable {
  mutating func binarySearch(for target: Element, in range: Range<Index>? = nil) -> Bool {
    let range = range ?? startIndex..<endIndex
    guard range.lowerBound < range.upperBound else { return false }
    
    let size = distance(from: range.lowerBound, to: range.upperBound)
    let middleIndex = index(range.lowerBound, offsetBy: size / 2)
    let middleValue = self[middleIndex]
    
    if target == middleValue {
      return true
    }
    
    if target > middleValue {
      return binarySearch(for: target, in: index(after: middleIndex)..<range.upperBound)
    }
    
    if target < middleValue {
      return binarySearch(for: target, in: range.lowerBound..<middleIndex)
    }
    
    return false
  }
}

var a = [1, 2, 3, 4, 5, 6, 7, 8, 9]
a.binarySearch(for: 10)



http://www.mczarnik.com/2017/04/08/break-out-of-reduce.html


// array duplicates

extension Sequence where Element: Hashable {
  func unique() -> [Element] {
    var seen: Set<Element> = []
    return filter { element in
      if seen.contains(element) {
        return false
      } else {
        seen.insert(element)
        return true
      }
    }
  }
}

class Solution {
  func removeDuplicates(_ nums: inout [Int]) -> Int {
    nums = nums.unique()
    return nums.count
  }
}

var a = [0,0,1,1,1,2,2,3,3,4]
Solution().removeDuplicates(&a)
print(a)

//Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory. the above solution meets these critera in swift because even if you manipulate points you are still creating a new array as the swapAt function is mutating https://developer.apple.com/documentation/swift/array/2893281-swapat
// but the seen set is an auxiliary data structure therefore this solution would probably not be accepted and you would want to use the pointers instead




# Linked List
```swift

class Node<Value> {
  let value: Value
  var next: Node?
  
  init(value: Value, next: Node? = nil) {
    self.value = value
    self.next = next
  }
}

extension Node: CustomStringConvertible {
  public var description: String {
    guard let next = next else {
      return "\(value)"
    }
    return "\(value) -> \(next)"
  }
}

struct LinkedList<Value> {
  var head: Node<Value>?
  var tail: Node<Value>?
  
  init() {}
  
  var isEmpty: Bool {
    return head == nil
  }
}

extension LinkedList: CustomStringConvertible {
  public var description: String {
    guard let head = head else {
      return ""
    }
    return "\(head)"
  }
}

extension LinkedList {
  mutating func push(_ value: Value) {
    head = Node(value: value, next: head)
    if tail == nil {
      tail = head
    }
  }
  
  mutating func append(_ value: Value) {
    guard !isEmpty else {
      push(value)
      return
    }
    tail?.next = Node(value: value)
    tail = tail?.next
  }
  
  @discardableResult
  mutating func insert(_ value: Value, after node: Node<Value>) -> Node<Value>? {
    guard tail !== node else {
      append(value)
      return tail
    }
    node.next = Node(value: value, next: node.next)
    return node.next
  }
  
  func node(at index: Int) -> Node<Value>? {
    var currentNode = head
    var currentIndex = 0
    
    while currentNode != nil && currentIndex < index {
      currentNode = currentNode?.next
      currentIndex += 1
    }
    return currentNode
  }
}

extension LinkedList {
  @discardableResult
  mutating func pop() -> Value? {
    defer {
      head = head?.next
      if isEmpty {
        tail = nil
      }
    }
    return head?.value
  }
  
  @discardableResult
  mutating func removeLast() -> Value? {
    guard let head = head else {
      return nil
    }
    
    guard head.next != nil else {
      return pop()
    }
    
    var current = head
    var previous = head
    
    while let next = current.next {
      previous = current
      current = next
    }
    
    previous.next = nil
    tail = previous
    return current.value
  }
  
  @discardableResult
  mutating func remove(after node: Node<Value>) -> Value? {
    defer {
      if tail === node.next {
        tail = node
      }
      node.next = node.next?.next
    }
    return node.next?.value
  }
}


```



```swift
// reverse a linked list iterative

func reverseList<Value>(_ head: Node<Value>?) -> Node<Value>? {
  var currentNode = head
  var previous: Node<Value>?
  var next: Node<Value>?
  
  while currentNode != nil {
    next = currentNode?.next
    currentNode?.next = previous
    previous = currentNode
    currentNode = next
  }
  return previous
}


```



## perfect squares

//Find of perfect squares between two numbers
// O(log n)
// O(1)


postfix operator **

extension Int {
  static postfix func **(number: Int) -> Int {
    return number * number
  }
}

extension CountableRange where Bound == Int {
  func perfectSquares() -> [Bound] {
    var result = [Bound]()
    
    for number in self where number** < upperBound {
      result.append(number**)
    }
    
    return result
  }
}

(1..<100).perfectSquares()



https://www.youtube.com/watch?v=MRe3UsRadKw





```swift
//Two Sum - Sorted - (Leetcode)
// Time O(n)
// Space O(1)

extension RandomAccessCollection where Element == Int {
  func twoSum(for target: Element) -> (Index, Index)? {
    guard count > 1 else { return nil }
    
    var left = startIndex
    var right = index(before: endIndex)

    while left < right {
      let total = self[left] + self[right]
      
      if total == target {
        return (left, right)
      }
      
      if total > target {
        right = index(before: right)
      }
      
      if total < target {
        left = index(after: left)
      }
    }
    return nil
  }
}

var a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
a.twoSum(for: 2)
```






//Reverse Linked List - Recursive
// Time O(n)
// Space O(n)

class Node<Value> {
  let value: Value
  var next: Node<Value>?
  
  init(_ value: Value, next: Node<Value>? = nil) {
    self.value = value
    self.next = next
  }
}

extension Node: CustomStringConvertible {
  public var description: String {
    guard let next = next else {
      return "\(value)"
    }
    return "\(value) -> \(next)"
  }
}

let node1 = Node(1)
let node2 = Node(2)
let node3 = Node(3)
let node4 = Node(4)

node1.next = node2
node2.next = node3
node3.next = node4

func reverseRecursive<Value>(_ node: Node<Value>?) -> Node<Value>? {
  guard let node = node else { return nil }
  guard node.next != nil else { return node }
  let head = reverseRecursive(node.next)
  node.next?.next = node
  node.next = nil
  return head
}

print(node1)
//print(reverseRecursive(node1))


//Reverse Linked List - Iter
// Time O(n)
// Space O(1)

func reverseIterative<Value>(_ node: Node<Value>?) -> Node<Value>? {
  var current = node
  var previous: Node<Value>? = nil
  var next: Node<Value>? = nil
  
  while current != nil {
    next = current?.next
    current?.next = previous
    previous = current
    current = next
  }
  return previous
}

print(reverseIterative(node1))







//Roman to Integer - (Leetcode)
// T O(n)
// S O(1)

let romanValues: [Character: Int] = ["I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000]

class Solution {
  func romanToInt(_ s: String) -> Int {
    var accumulator = 0
    
    for index in s.indices {
      var romanValue = value(for: s[index])
      
      let nextIndex = s.index(after: index)
      if nextIndex < s.endIndex, value(for: s[nextIndex]) > romanValue {
        romanValue *= -1
      }

      accumulator += romanValue
    }
    
    return accumulator
  }
  
  func value(for character: Character) -> Int {
    return romanValues[character] ?? 0
  }
}

Solution().romanToInt("XXIV")







//Move Zeroes - (Leetcode)
// T O(n)
// S O(1)

class Solution {
  func moveZeroes(_ nums: inout [Int]) {
    var i = 0
    
    for j in 1..<nums.count {
      if nums[i] != nums[j] {
        nums.swapAt(i, j)
        i += 1
      }
    }
  }
}

var a = [0,1,0,3,12]
Solution().moveZeroes(&a)











class Solution {
  func maxSubArray(_ nums: [Int]) -> Int {
    guard nums.count > 0 else { return 0 }
    guard nums.count > 1 else { return nums[0] }
    
    var currentMax = Int.min
    var globalMax = Int.min
    
    for num in nums {
      currentMax = num + max(currentMax, 0)
      globalMax = max(currentMax, globalMax)
    }
    return globalMax
  }
}


Solution().maxSubArray([-2,1,-3,4,-1,2,1,-5,4])






struct UnfoldingSequence<State, T>: Sequence, IteratorProtocol {
  var state: State
  let _next: (inout State) -> T?
  
  init(state: State, next: @escaping (inout State) -> T?) {
    self.state = state
    _next = next
  }
  
  mutating func next() -> T? {
    print(state)
    return _next(&state)
  }
}

let fibs = UnfoldingSequence(state: (0, 1)) { (state) -> Int? in
  defer { state = (state.1, state.0 + state.1) }
  return state.0
}

let r = AnySequence(fibs.prefix(10).map { $0 * 2 })


https://github.com/raywenderlich/swift-algorithm-club/tree/master/Combinatorics

Time - O(n)
Space - O(n)

func factorialR(_ num: Int) -> Int {
  guard num > 1 else { return 1 }
  return num * factorialR(num - 1)
}

Time - O(n)
Space - O(1)

func factorialI(_ num: Int) -> Int {
  guard num > 1 else { return 1 }
  
  var num = num
  var result = 1
  
  while num > 1 {
    result *= num
    num -= 1
  }
  return result
}

factorialR(3)


var fibonacciArray:[Int] = [0,1]

var fibonacciTest = 6

func fibonacci(_ n: Int) -> Int {
  if (fibonacciArray.count > n) {
    return fibonacciArray[n]
  }
  
  let fibonacciNumber = fibonacci(n - 1) + fibonacci(n - 2)
  fibonacciArray.append(fibonacciNumber)
  return fibonacciNumber
}

fibonacci(fibonacciTest)




extension CountableRange where Bound == Int {
  func perfectSquares() -> [Bound] {
    var result = [Bound]()
    
    var current = lowerBound
    
    while current * current < upperBound {
      result.append(current * current)
      current += 1
    }
    
    return result
  }
}

(0..<100).perfectSquares()
















// Linked List

class Node<Value> {
  let value: Value
  var next: Node<Value>?
  
  init(_ value: Value, next: Node<Value>? = nil) {
    self.value = value
    self.next = next
  }
}

extension Node: CustomStringConvertible {
  public var description: String {
    guard let next = next else {
      return "\(value)"
    }
    return "\(value) -> \(next)"
  }
}

struct LinkedList<Value> {
  var head: Node<Value>?
  var tail: Node<Value>?
  
  init() {}
  
  var isEmpty: Bool {
    return head == nil
  }
}

extension LinkedList: CustomStringConvertible {
  public var description: String {
    guard let head = head else {
      return ""
    }
    return "\(head)"
  }
}

extension LinkedList {
  mutating func push(_ value: Value) {
    head = Node(value, next: head)
    if tail == nil {
      tail = head
    }
  }
  
  mutating func append(_ value: Value) {
    guard !isEmpty else {
      push(value)
      return
    }
    tail?.next = Node(value)
    tail = tail?.next
  }
  
  mutating func insert(_ value: Value, after node: Node<Value>?) {
    guard tail !== node else {
      append(value)
      return
    }
    
    node?.next = Node(value, next: node?.next)
    
  }
  
  func node(at index: Int) -> Node<Value>? {
    var currentNode = head
    var currentIndex = 0
    
    while currentNode != nil && currentIndex < index {
      currentNode = currentNode?.next
      currentIndex += 1
    }
    
    return currentNode
  }
}

extension LinkedList {
  @discardableResult
  mutating func pop() -> Node<Value>? {
    defer {
      head = head?.next
      if isEmpty {
        tail = nil
      }
    }
    return head
  }
  
  @discardableResult
  mutating func removeLast() -> Node<Value>? {
    guard let head = head else { return nil }
    guard head.next != nil else { return pop() }
    
    var current = head
    var previous = head
    
    while let next = current.next {
      previous = current
      current = next
    }
    
    previous.next = nil
    tail = previous
    return current
  }
  
  @discardableResult
  mutating func remove(after node: Node<Value>?) -> Node<Value>? {
    defer {
      if node?.next === tail {
        tail = node
      }
      node?.next = node?.next?.next
    }
    return node?.next
  }
}

extension LinkedList {
  struct Index: Comparable {
    var node: Node<Value>?
    
    static func <(lhs: Index, rhs: Index) -> Bool {
      guard lhs != rhs else { return false }
      
      let seq = sequence(first: lhs.node?.next) { $0?.next }
      return seq.contains { $0 === rhs.node }
    }
    
    static func ==(lhs: Index, rhs: Index) -> Bool {
      switch (lhs.node, rhs.node) {
      case let (left?, right?):
        return left === right
      case (nil, nil):
        return true
      default:
        return false
      }
    }
  }
}

extension LinkedList: Collection {
  var startIndex: Index {
    return Index(node: head)
  }
  
  var endIndex: Index {
    return Index(node: tail?.next)
  }
  
  func index(after i: Index) -> Index {
    return Index(node: i.node?.next)
  }
  
  subscript(_ index: Index) -> Value {
    return index.node!.value
  }
}

# Print Linked List in reverse

extension LinkedList {
  func printReverseR(_ node: Node<Value>?) {
    guard node != nil else { return }
    printReverseR(node?.next)
    print(node!.value)
  }
  
  func printReverseI() {
    var stack = Stack<Value>()
    
    var current = head
    while let next = current {
      stack.push(next.value)
      current = next.next
    }
    
    while let node = stack.pop() {
      print(node)
    }
  }
  
  func printReverseS() -> String {
    var result = ""
    var current = head
    while let next = current {
      current = current?.next
      result = "\(next.value)" + result
    }
    return result
  }
}

struct Stack<Element> {
  private var elements: [Element] = []
  
  mutating func pop() -> Element? {
    return elements.popLast()
  }
  
  mutating func push(_ element: Element) {
    return elements.append(element)
  }
  
}

# middle node of linked list

//"Create a function that returns the middle node of a linked list",

import Darwin

extension LinkedList {
  func middle() -> Node<Value> {
    var current = head
    var counter = 0
    
    while current != nil {
      current = current?.next
      counter += 1
    }
    
    let middleIndex = counter / 2
    current = head
    counter = 0
    
    while current != nil && counter < middleIndex {
      current = current?.next
      counter += 1
    }
    
    return current!
  }
}


//"Create a function that returns the middle node of a linked list",

import Darwin

extension LinkedList {
  func middle() -> Node<Value> {
    var slowPointer = head
    var fastPointer = head
    
    while fastPointer?.next != nil {
      slowPointer = slowPointer?.next
      fastPointer = fastPointer?.next?.next
    }
    
    return slowPointer!
  }
}


//"Create a function that returns the middle node of a linked list",

import Darwin

extension LinkedList {
  func middle() -> Node<Value> {
    var counter = 0
    var current = head
    var mid = head
    
    while current != nil {
      if counter % 2 != 0 {
        mid = mid?.next
      }
      
      current = current?.next
      counter += 1
    }
    
    return mid!
  }
}

struct Stack<Element> {
  private var elements: [Element] = []
  
  mutating func pop() -> Element? {
    return elements.popLast()
  }
  
  mutating func push(_ element: Element) {
    return elements.append(element)
  }
  
}

var ll = LinkedList<Int>()
ll.append(1)
ll.append(2)
ll.append(3)
ll.append(4)
ll.append(3)
ll.append(4)
ll.middle().value




extension LinkedList {
  func reverseI() -> Node<Value>? {
    var current = head
    var previous: Node<Value>?
    var next: Node<Value>?
    
    while current != nil {
      next = current?.next
      current?.next = previous
      previous = current
      current = next
    }
    return previous
  }
  
  func reverseR(_ node: Node<Value>?) -> Node<Value>? {
    guard let node = node else { return nil }
    guard node.next != nil else { return node }
    let head = reverseR(node.next)
    node.next?.next = node
    node.next = nil
    return head
  }
}



//Binary Search - Recursive
// O(log n)
// O(log n)

extension RandomAccessCollection where Element: Comparable {
  func binarySearch(for target: Element, in range: Range<Index>? = nil) -> Bool {
    let range = range ?? startIndex..<endIndex
    guard range.lowerBound < range.upperBound else { return false }
    
    let size = distance(from: range.lowerBound, to: range.upperBound)
    let middleIndex = index(range.lowerBound, offsetBy: size / 2)
    let middleValue = self[middleIndex]
    
    if middleValue == target {
      return true
    }
    
    if middleValue > target {
      return binarySearch(for: target, in: range.lowerBound..<middleIndex)
    }
    
    if middleValue < target {
      return binarySearch(for: target, in: index(after: middleIndex)..<range.upperBound)
    }
    return false
  }
}

var a = [1, 2, 3, 4, 5, 6, 7, 8, 9]
a.binarySearch(for: 10)



// Jewels and Stones - custom operators

extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return reduce(0) { predicate($1) ? $0 + 1 : $0 }
  }
}

class Solution {
  func numJewelsInStones(_ J: String, _ S: String) -> Int {
    return S.count(where: J.contains)
  }
}



// Jewels and Stones - custom operators

infix operator ~

extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return reduce(0) { predicate($1) ? $0 + 1 : $0 }
  }
}

extension Sequence where Element: Hashable {
  func occurences(in other: Self) -> Int {
    let elementSet = Set(self)
    return other.count(where: elementSet.contains)
  }
  
  static func ~(lhs: Self, rhs: Self) -> Int {
    return lhs.occurences(in: rhs)
  }
}

class Solution {
  func numJewelsInStones(_ J: String, _ S: String) -> Int {
    return J ~ S
  }
}

Solution().numJewelsInStones("aA", "aAAbbbb")






extension Sequence where Element: Hashable {
  var frequencies: [Element: Int] {
    let frequencyPairs = self.map { ($0, 1) }
    return Dictionary(frequencyPairs, uniquingKeysWith: +)
  }
}

let frequencies = "hello".frequencies // ["e": 1, "o": 1, "l": 2, "h": 1] frequencies. lter { $0.value > 1 } // ["l": 2]






extension Sequence where Element: Hashable {
  func unique() -> [Element] {
    var set = Set<Element>()
    return filter { element in
      if set.contains(element) {
        return false
      } else {
        set.insert(element)
        return true
      }
    }
  }
}

[1, 1, 2, 3, 3, 4, 5, 6, 6, 6, 7, 8, 8].unique()





//LinkedList


//"Create a function that prints out the elements of a linked list in reverse order",


//"Create a function that returns the middle node of a linked list",


//"Create a function that reverses a linked list",


//"Create a function that takes two sorted linked lists and merges them into a single sorted linked list",


//"Create a function that removes all occurrences of a specific element from a linked list",


//"Delete node in a linked list",


//"Remove linked list elements",


//"Intersection of two linked lists",


//"Linked list cycle",


//"Palindrome linked list"





// Challenge 1: Are the letters unique?
func challenge1(input: String) -> Bool {
  return input.count == Set(input).count
}

assert(challenge1(input: "No duplicates") == true, "Challenge 1 failed")
assert(challenge1(input: "abcdefghijklmnopqrstuvwxyz") == true, "Challenge 1 failed")
assert(challenge1(input: "AaBbCc") == true, "Challenge 1 failed")
assert(challenge1(input: "Hello, world") == false, "Challenge 1 failed")



// Challenge 2: Is a string a palindrome?

extension String {
  func palindrome() -> Bool {
    let lowercased = self.lowercased()
    return lowercased == String(lowercased.reversed())
  }
}










// Challenge 2: Is a string a palindrome?


extension String {
  subscript(position: Int) -> Character {
    return self[index(startIndex, offsetBy: position)]
  }
}

"Hello"[1]

extension String {

  func palindrome1() -> Bool {
    let lowercased = self.lowercased()
    return lowercased == String(lowercased.reversed())
  }

  func palindrome2() -> Bool {
    let lowercased = self.lowercased()
    var reversedString = ""
    for character in lowercased {
      reversedString = "\(character)" + reversedString
    }
    return reversedString == lowercased
  }

  func palindrome3() -> Bool {
    let lowercased = self.lowercased()
    var reversedString = ""

    var stack = Stack<Character>()
    for character in self {
      stack.push(character)
    }

    while let char = stack.pop() {
      reversedString += "\(char)"
    }
    return reversedString == lowercased
  }

  func palindrome4() -> Bool {
    let lengthHalf = count / 2
    var lengthOne = count - 1

    var i = 0
    while i < lengthHalf {
      if self[i] != self[lengthOne] {
        return false
      }
      i += 1
      lengthOne -= 1
    }
    return true
  }


  //"Challenge 2: Is a string a palindrome?",

extension String {
  func pal1() -> Bool {
    var left = startIndex
    var right = index(before: endIndex)
    
    while left <= right {
      if self[left] != self[right] {
        return false
      }
      left = index(after: left)
      right = index(before: right)
    }
    return true
  }
}

"racecar".pal1()

  
}

extension String {
  func palindrome4() -> Bool {
    let size = distance(from: startIndex, to: endIndex)
    let middleIndex = index(startIndex, offsetBy: size / 2)
    
    var leftIndex = startIndex
    var rightIndex = index(before: endIndex)
    
    while leftIndex < middleIndex {
      if self[leftIndex] != self[rightIndex] {
        return false
      }
      leftIndex = index(after: leftIndex)
      rightIndex = index(before: rightIndex)
    }
    return true
  }
}

struct Stack<Value> {
  var elements = [Value]()

  mutating func push(_ value: Value) {
    elements.append(value)
  }

  mutating func pop() -> Value? {
    return elements.popLast()
  }
}

"racecar".palindrome4()




// Challenge 3: Do two strings contain the same characters?

func sameCharacters(_ string1: String, _ string2: String) -> Bool {
  let array1 = Array(string1)
  let array2 = Array(string2)
  return array1.sorted() == array2.sorted()
}


//For bonus interview points, you might be tempted to write something like this:
//
//return array1.count == array2.count && array1.sorted() ==
//  array2.sorted()
//
//However, there’s no need – the == operator for arrays does that before doing its item-by-item comparison, so doing it yourself is just redundant.
// conformance to the equatable protocol makes this check in a guard on the static == func

//"Challenge 5: Count the characters"


import Foundation

extension String {
  func fuzzyContains(_ substring: String) -> Bool {
    return range(of: substring, options: .caseInsensitive) != nil
  }
}

"Hello, world".fuzzyContains("Hello")

// Challenge 5: Count the characters

import Foundation

extension String {
  func count1(for character: Character) -> Int {
    var count = 0
    
    for element in self {
      if element == character {
        count += 1
      }
    }
    
    return count
  }
  
  func count2(for character: Character) -> Int {
    let set = NSCountedSet(array: Array(self))
    return set.count(for: character)
  }

  func count3(for character: Character) -> Int {
    return reduce(0) { $1 == character ? $0 + 1 : $0 }
  }

  func count4(for character: String) -> Int {
    let modified = replacingOccurrences(of: character, with: "")
    return count - modified.count
  }
}

"Hello".count4(for: "l")

func challenge5d(input: String, count: String) -> Int {
   let modified = input.replacingOccurrences(of: count, with:
"")
   return input.count - modified.count
}


struct UnfoldingSequence<State, T>: Sequence, IteratorProtocol {
  let _next: (inout State) -> T?
  var state: State
  
  init(state: State, next: @escaping (inout State) -> T?) {
    _next = next
    self.state = state
  }
  
  mutating func next() -> T? {
    return _next(&state)
  }
}

let fibs = UnfoldingSequence(state: (0, 1)) { state -> Int? in
  defer { state = (state.1, state.0 + state.1) }
  return state.0
}

for f in fibs.prefix(10) {
  print(f)
}


extension String {
  func unique() -> String {
    var seenElements = Set<Character>()
    return filter { element in
      if seenElements.contains(element) {
        return false
      } else {
        seenElements.insert(element)
        return true
      }
    }
  }
}

"Mississippi".unique()



extension String {
  func unique() -> String {
    var seenElements = Set<Character>()
    return reduce("") { result, element in
      if seenElements.contains(element) {
        return result
      } else {
        seenElements.insert(element)
        return result + "\(element)"
      }
    }
  }
}

"Hello".unique()



//"Create a function that prints out the elements of a linked list in reverse order",

struct Stack<Value> {
  var elements: [Value] = []
  
  mutating func push(_ value: Value) {
    elements.append(value)
  }
  
  mutating func pop() -> Value? {
    return elements.popLast()
  }
}

//func r<Value>(_ node: Node<Value>?) {
//  var stack = Stack<Value>()
//  var current = node
//
//  while current != nil {
//    stack.push(current!.value)
//    current = current!.next
//  }
//
//  while let value = stack.pop() {
//    print(value)
//  }
//}

//func r<Value>(_ node: Node<Value>?) -> String {
//  var current = node
//  var result = ""
//  while current != nil {
//    result = "\(current!.value)" + " " + result
//    current = current!.next
//  }
//  return result
//}

func r<Value>(_ node: Node<Value>?) {
  guard node != nil else { return }
  r(node?.next)
  print(node!.value)
}

r(node4)


//"Create a function that returns the middle node of a linked list",

func middle<Value>(_ node: Node<Value>?) -> Node<Value>? {
  var current = node
  var previous = node
  
  while current != nil {
    current = current?.next?.next
    previous = previous?.next
  }
  return previous?.next
}

middle(node4)



func middle<Value>(_ node: Node<Value>?) -> Node<Value>? {
  var current = node
  var previous = node
  var count = 0
  
  while current != nil {
    count += 1
    
    if count % 2 != 0 {
      previous = previous?.next
    }
    
    current = current?.next
    
  }
  return previous
}

middle(node4)









//"Remove linked list elements",

class Solution {
  func removeElements(_ head: ListNode?, _ val: Int) -> ListNode? {
    var current = head
    var previous: ListNode?
    
    while current != nil {
      if current?.val == val {
        previous?.next = current?.next
      }
      previous = current
      current = current?.next
    }
    return head
  }
  
  func removeElementsR(_ head: ListNode?, _ val: Int) -> ListNode? {
    if head == nil {
      return nil
    }
    
    head?.next = removeElementsR(head, val)
    
    return head?.val == val ? head?.next : head
  }
}

Solution().removeElements(node1, 6)


//"Delete node in a linked list",

func delete(_ head: ListNode?, _ val: Int) {
  var current = head
  var previous = head
  
  while current != nil {
    if current?.val == val {
      previous?.next = current?.next
      return
    }
    previous = current
    current = current?.next
  }
}

print(node1)
delete(node1, 3)
print(node1)



//"Linked list cycle",

func cycle(_ node: ListNode?) -> Bool {
  var slow = node
  var fast = node
  
  while slow !== fast {
    if fast == nil || fast?.next == nil {
      return false
    }
    fast = fast?.next?.next
    slow = slow?.next
  }
  return true
}

cycle(node1)