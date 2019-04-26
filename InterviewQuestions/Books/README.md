# Stacks and Queues

## Three In One

```swift
struct MultiStack<Element> {
    var base: [Element?] = []
    var sizes: [Int] = []
    
    var numberOfStacks: Int
    var capacity: Int
    
    init(with numberOfStacks: Int, capacity: Int) {
        base = [Element?](repeating: nil, count: numberOfStacks * capacity)
        sizes = [Int](repeating: 0, count: numberOfStacks)
        
        self.numberOfStacks = numberOfStacks
        self.capacity = capacity
    }
    
    mutating func push(_ element: Element, onTo stackNumber: Int) {
        guard !isFull(stackNumber) else {
            return
        }
        sizes[stackNumber] += 1
        base[indexOfTop(stackNumber)] = element
    }

    mutating func pop(from stackNumber: Int) -> Element? {
        guard !isEmpty(in: stackNumber) else {
            return nil
        }
        
        defer {
            base[indexOfTop(stackNumber)] = nil
            sizes[stackNumber] -= 1
        }
        return base[indexOfTop(stackNumber)]
    }

    mutating func peek(on stackNumber: Int) -> Element? {
        return base[indexOfTop(stackNumber)]
    }

    mutating func isEmpty(in stackNumber: Int) -> Bool {
        return sizes[stackNumber] == 0
    }

    func isFull(_ stackNumber: Int) -> Bool {
        return sizes[stackNumber] == capacity
    }

    func indexOfTop(_ stackNumber: Int) -> Int {
        let offset = stackNumber * capacity
        let size = sizes[stackNumber]
        return offset + size - 1
    }
}

var stack = MultiStack<Int>(with: 3, capacity: 5)
stack.push(1, onTo: 0)
stack.push(2, onTo: 1)
stack.push(3, onTo: 2)
stack.push(4, onTo: 0)
stack.push(5, onTo: 2)
stack.push(6, onTo: 2)
stack.peek(on: 0)
stack.peek(on: 1)
print(stack)
stack.peek(on: 2)
print(stack)
stack.pop(from: 2)
print(stack)
stack.pop(from: 2)
print(stack)
stack.pop(from: 2)
print(stack)
stack.pop(from: 2)
print(stack)
stack.pop(from: 2)
print(stack)

```

## Stack of Plates

```swift
class Stack<Element> {
    var elements: [Element] = []
    var capacity: Int
    
    init(capacity: Int) {
        elements.reserveCapacity(capacity)
        self.capacity = capacity
    }
    
    var count: Int {
        return elements.count
    }
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    var isFull: Bool {
        return elements.count >= capacity
    }
    
    func push(_ element: Element) {
        elements.append(element)
    }
    
    func pop() -> Element? {
        return elements.popLast()
    }
}

extension Stack: CustomStringConvertible {
    var description: String {
        return "\(elements)"
    }
}

struct SetOfStacks<Element> {
    var stacks: [Stack<Element>] = []
    var capacity: Int
    
    init(capacity: Int) {
        self.capacity = capacity
    }
    
    mutating func push(_ element: Element) {
        guard let stack = stacks.last, !stack.isFull else {
            let stack = Stack<Element>(capacity: capacity)
            stack.push(element)
            stacks.append(stack)
            return
        }
        stack.push(element)
    }
    
    mutating func pop() -> Element? {
        guard let stack = stacks.last else {
            return nil
        }
        
        guard stack.isEmpty else {
            return stack.pop()
        }
        
        stacks.popLast()
        return pop()
    }
    
    mutating func pop(at index: Int) -> Element? {
        guard index < stacks.count else {
            return nil
        }
        return stacks[index].pop()
    }
}

extension SetOfStacks: CustomStringConvertible {
    var description: String {
        return "\(stacks)"
    }
}

var stacks = SetOfStacks<Int>(capacity: 5)
stacks.push(1)
stacks.push(2)
stacks.push(3)
stacks.push(4)
stacks.push(5)
stacks.push(6)
stacks.push(7)
stacks.push(8)
stacks.push(9)
stacks.pop()
stacks.pop()
print(stacks)
stacks.pop(at: 0)
print(stacks)
stacks.pop(at: 0)
print(stacks)
stacks.pop()
stacks.pop()
print(stacks)
stacks.pop()
stacks.pop()
stacks.pop()
stacks.pop()

```

## Queue via Stack

```swift
struct Stack<Element> {
    var elements: [Element] = []
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}

struct Queue<Element> {
    var leftStack = Stack<Element>()
    var rightStack = Stack<Element>()
    
    mutating func enqueue(_ element: Element) {
        leftStack.push(element)
    }
    
    mutating func dequeue() -> Element? {
        if rightStack.isEmpty {
            while let value = leftStack.pop() {
                rightStack.push(value)
            }
        }
        return rightStack.pop()
    }
}

extension Queue: ExpressibleByArrayLiteral {
    init(arrayLiteral: Element...) {
        for element in arrayLiteral {
            leftStack.push(element)
        }
    }
}

var queue: Queue = [1, 2, 3, 4, 5]
queue.dequeue()
queue.dequeue()
queue.dequeue()
queue.dequeue()
queue.dequeue()
queue.dequeue()

```

## Sort Stack

```swift
struct Stack<Element> {
    var elements: [Element] = []
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    func peek() -> Element? {
        return elements.last
    }
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}

struct SortedStack<Element: Comparable> {
    var base = Stack<Element>()
    
    mutating func push(_ element: Element) {
        base.push(element)
    }
    
    mutating func pop() -> Element? {
        return base.pop()
    }
    
    mutating func peek() -> Element? {
        return base.peek()
    }

    mutating func sort() {
        var bufferStack = Stack<Element>()
        
        while !base.isEmpty {
            guard let temp = base.pop() else {
                return
            }
            
            while let last = bufferStack.peek(), last > temp {
                base.push(bufferStack.pop()!)
            }
            
            bufferStack.push(temp)
        }
        
        while let value = bufferStack.pop() {
            base.push(value)
        }
        
    }

    var isEmpty: Bool {
        return base.isEmpty
    }
}


var sortedStack = SortedStack<Int>()
sortedStack.push(10)
sortedStack.push(1)
sortedStack.push(5)
sortedStack.push(6)
sortedStack.push(20)
sortedStack.push(3)
print(sortedStack)
sortedStack.sort()
print(sortedStack)

```

## Animal Queue

```swift
import Foundation

struct Stack<Element> {
    var elements: [Element] = []
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    func peek() -> Element? {
        return elements.last
    }
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}

struct Queue<Element> {
    var leftStack = Stack<Element>()
    var rightStack = Stack<Element>()
    
    mutating func peek() -> Element? {
        shiftStacks()
        return rightStack.peek()
    }
    
    var isEmpty: Bool {
        return leftStack.isEmpty && rightStack.isEmpty
    }
    
    mutating func enqueue(_ element: Element) {
        leftStack.push(element)
    }
    
    mutating func dequeue() -> Element? {
        shiftStacks()
        return rightStack.pop()
    }
    
    mutating func shiftStacks() {
        if rightStack.isEmpty {
            while let value = leftStack.pop() {
                rightStack.push(value)
            }
        }
    }
}

struct AnimalQueue {
    var dogQueue = Queue<AnimalQueueNode>()
    var catQueue = Queue<AnimalQueueNode>()
    
    mutating func enqueue(_ animal: Animal) {
        let node = AnimalQueueNode(animal)
        if animal is Dog {
            dogQueue.enqueue(node)
        } else {
            catQueue.enqueue(node)
        }
    }
    
    mutating func dequeueAny() -> Animal? {
        guard let dogNode = dogQueue.peek() else {
            return dequeueCat()
            
        }
        
        guard let catNode = catQueue.peek() else {
            return dequeueDog()
        }
        
        if dogNode.enqueuedDate > catNode.enqueuedDate {
            return dequeueCat()
        } else {
            return dequeueDog()
        }
    }
    
    mutating func dequeueDog() -> Dog? {
        return dogQueue.dequeue()?.animal as? Dog
    }
    
    mutating func dequeueCat() -> Cat? {
        return catQueue.dequeue()?.animal as? Cat
    }
}

struct AnimalQueueNode {
    var animal: Animal
    var enqueuedDate: Date
    
    init(_ animal: Animal) {
        self.animal = animal
        enqueuedDate = Date()
    }
}

protocol Animal {
    var name: String { get }
}

struct Cat: Animal {
    var name: String
}

extension Cat: CustomStringConvertible {
    var description: String {
        return "\(name)"
    }
}

struct Dog: Animal {
    var name: String
}

extension Dog: CustomStringConvertible {
    var description: String {
        return "\(name)"
    }
}

var animals = AnimalQueue()
animals.enqueue(Dog(name: "Roy"))
animals.enqueue(Dog(name: "Spot"))
animals.enqueue(Cat(name: "Jake"))
animals.enqueue(Dog(name: "Sara"))
animals.enqueue(Dog(name: "Skip"))
animals.enqueue(Dog(name: "Toy"))
animals.enqueue(Cat(name: "Roger"))
animals.enqueue(Cat(name: "Emily"))
animals.enqueue(Cat(name: "Don"))

print(animals)

animals.dequeueAny()
animals.dequeueDog()
animals.dequeueDog()
animals.dequeueDog()
animals.dequeueAny()
animals.dequeueAny()
animals.dequeueCat()
animals.dequeueCat()
animals.dequeueDog()
animals.dequeueAny()

```


struct MultiStack<Element> {
    var base: [Element?] = []
    var sizes: [Int] = []
    
    var numberOfStacks: Int
    var capacity: Int
    
    init(with numberOfStacks: Int, capacity: Int) {
        base = [Element?](repeating: nil, count: numberOfStacks * capacity)
        sizes = [Int](repeating: 0, count: numberOfStacks)
        
        self.numberOfStacks = numberOfStacks
        self.capacity = capacity
    }
    
    mutating func push(_ element: Element, onTo stackNumber: Int) {
        guard !isFull(stackNumber) else {
            return
        }
        sizes[stackNumber] += 1
        base[indexOfTop(stackNumber)] = element
    }

    mutating func pop(from stackNumber: Int) -> Element? {
        guard !isEmpty(in: stackNumber) else {
            return nil
        }
        
        defer {
            base[indexOfTop(stackNumber)] = nil
            sizes[stackNumber] -= 1
        }
        return base[indexOfTop(stackNumber)]
    }

    mutating func peek(on stackNumber: Int) -> Element? {
        return base[indexOfTop(stackNumber)]
    }

    mutating func isEmpty(in stackNumber: Int) -> Bool {
        return sizes[stackNumber] == 0
    }

    func isFull(_ stackNumber: Int) -> Bool {
        return sizes[stackNumber] == capacity
    }

    func indexOfTop(_ stackNumber: Int) -> Int {
        let offset = stackNumber * capacity
        let size = sizes[stackNumber]
        return offset + size - 1
    }
}

var stack = MultiStack<Int>(with: 3, capacity: 5)
stack.push(1, onTo: 0)
stack.push(2, onTo: 1)
stack.push(3, onTo: 2)
stack.push(4, onTo: 0)
stack.push(5, onTo: 2)
stack.push(6, onTo: 2)
stack.peek(on: 0)
stack.peek(on: 1)
print(stack)
stack.peek(on: 2)
print(stack)
stack.pop(from: 2)
print(stack)
stack.pop(from: 2)
print(stack)
stack.pop(from: 2)
print(stack)
stack.pop(from: 2)
print(stack)
stack.pop(from: 2)
print(stack)



## Mth to last node

```swift
class Node<Value> {
    let value: Value
    var next: Node?
    
    init(_ value: Value, next: Node? = nil) {
        self.value = value
        self.next = next
    }
}

extension Node: CustomStringConvertible {
    var description: String {
        guard let next = next else {
            return "\(value)"
        }
        return "\(value) -> \(next)"
    }
}

struct LinkedList<Element> {
    var head: Node<Element>?
    var tail: Node<Element>?
    
    var isEmpty: Bool {
        return head == nil
    }
}

extension LinkedList {
    mutating func push(_ element: Element) {
        head = Node(element, next: head)
        if tail == nil {
            tail = head
        }
    }
    
    mutating func append(_ element: Element) {
        guard head !== nil else {
            push(element)
            return
        }
        tail?.next = Node(element)
        tail = tail?.next
    }

    func kToLast(_ k: Int) -> Node<Element>? {
        guard k >= 0 else {
            return nil
        }
    
        var fast: Node<Element>? = head
        var slow: Node<Element>? = head
        
        for _ in 0..<k {
            fast = fast?.next
        }
        
        guard fast != nil else {
            return nil
        }

        while fast?.next != nil {
            fast = fast?.next
            slow = slow?.next
        }
        return slow
    }
}

extension LinkedList: ExpressibleByArrayLiteral {
    init(arrayLiteral: Element...) {
        for element in arrayLiteral {
            append(element)
        }
    }
}

var linkedList: LinkedList<Int> = [1, 2, 3, 4, 5, 6]
linkedList.last(6)

```

## delete and insert after

```swift
class Node<Value> {
    let value: Value
    var next: Node?
    
    init(_ value: Value, next: Node? = nil) {
        self.value = value
        self.next = next
    }
}

extension Node: CustomStringConvertible {
    var description: String {
        guard let next = next else {
            return "\(value)"
        }
        return "\(value) -> \(next)"
    }
}

struct LinkedList<Element> {
    var head: Node<Element>?
    var tail: Node<Element>?
}

extension LinkedList {
    mutating func push(_ element: Element) {
        head = Node(element, next: head)
        if tail == nil {
            tail = head
        }
    }
    
    mutating func pop() -> Element? {
        defer {
            head = head?.next
        }
        return head?.value
    }
    
    mutating func delete(_ node: Node<Element>?) {
        guard node !== head else {
            head = head?.next
            return
        }
        
        var current = head
        
        while current?.next != nil {
            if current?.next === node {
                break
            }
            
            current = current?.next
        }
        
        if current?.next === tail {
            tail = current
        }
        
        current?.next = current?.next?.next
    }
    
    func node(at index: Int) -> Node<Element>? {
        var currentNode = head
        var currentIndex = 0
        
        while currentNode != nil && currentIndex < index {
            currentNode = currentNode?.next
            currentIndex += 1
        }
        return currentNode
    }
    
    mutating func insert(_ element: Element, after node: Node<Element>?) {
        guard node !== tail else {
            append(element)
            return
        }
        node?.next = Node(element, next: node?.next)
    }
    
    mutating func append(_ element: Element) {
        guard head != nil else {
            push(element)
            return
        }
        tail?.next = Node(element)
        tail = tail?.next
    }
}

extension LinkedList: Sequence {
    func makeIterator() -> LinkedListIterator<Element> {
        return LinkedListIterator(head)
    }
}

struct LinkedListIterator<Element>: IteratorProtocol {
    var current: Node<Element>?
    
    init(_ start: Node<Element>?) {
        current = start
    }
    
    mutating func next() -> Element? {
        defer {
            current = current?.next
        }
        return current?.value
    }
}

extension LinkedList: ExpressibleByArrayLiteral {
    init(arrayLiteral: Element...) {
        for element in arrayLiteral.reversed() {
            push(element)
        }
    }
}

extension LinkedList: CustomStringConvertible {
    var description: String {
        guard let head = head else {
            return ""
        }
        return "\(head)"
    }
}

var linkedList: LinkedList<Int> = [1, 2, 3, 4, 5]
let node = linkedList.node(at: 4)
linkedList.insert(99, after: node)
linkedList.head
linkedList.tail

```


## Merging Meeting Times

```swift

// Time: O(n log n)
// Space - O(n)

class Meeting: CustomStringConvertible {
    var startTime: Int
    var endTime: Int

    init(startTime: Int, endTime: Int) {
        self.startTime = startTime
        self.endTime = endTime
    }

    var description: String {
        return "(\(startTime), \(endTime))"
    }
}

extension Array where Element == Meeting {
    func mergeRanges() -> [Meeting] {
        guard !isEmpty else { return [] }
        
        let sortedMeetings = sorted { $0.startTime < $1.startTime }
        var mergedMeetings = [sortedMeetings[0]]

        for meeting in sortedMeetings.dropFirst() {
            guard let lastMergedMeeting = mergedMeetings.last else { break }

            if meeting.startTime <= lastMergedMeeting.endTime {
                lastMergedMeeting.endTime = Swift.max(lastMergedMeeting.endTime, meeting.endTime)
            } else {
                mergedMeetings.append(meeting)
            }
        }
        return mergedMeetings
    }
}


let meetings =   [
    Meeting(startTime: 0,  endTime: 1),
    Meeting(startTime: 3,  endTime: 5),
    Meeting(startTime: 4,  endTime: 8),
    Meeting(startTime: 10, endTime: 12),
    Meeting(startTime: 9,  endTime: 10)
]

print(meetings.mergeRanges())

```

## Reverse String in Place

```swift

// Time: O(n)
// Space - O(1)

extension String {
    mutating func reverse() {
        var leftIndex = startIndex
        var rightIndex = index(before: endIndex)
        
        while leftIndex < rightIndex {
            let first = self[leftIndex]
            let second = self[rightIndex]

            replaceSubrange(leftIndex...leftIndex, with: String(second))
            replaceSubrange(rightIndex...rightIndex, with: String(first))

            leftIndex = index(after: leftIndex)
            rightIndex = index(before: rightIndex)
        }
    }
}

var a = "Taco"
a.reverse()

```

## Reverse Words

```swift

// Time: O(n)
// Space - O(1)

func reverseCharacters(_ string: inout String, from startIndex: String.Index, until endIndex: String.Index) {
    guard startIndex != endIndex else { return }

    var leftIndex  = startIndex
    var rightIndex = string.index(before: endIndex)

    while leftIndex < rightIndex {
        let leftChar  = string[leftIndex]
        let rightChar = string[rightIndex]

        string.replaceSubrange(leftIndex...leftIndex, with: String(rightChar))
        string.replaceSubrange(rightIndex...rightIndex, with: String(leftChar))

        leftIndex  = string.index(after: leftIndex)
        rightIndex = string.index(before: rightIndex)
    }
}

func reverseWords(_ message: inout String) {
    reverseCharacters(&message, from: message.startIndex, until: message.endIndex)
    
    var currentWordStartIndex = message.startIndex
    
    for i in message.indices {
        if message[i] == " " {
            reverseCharacters(&message, from: currentWordStartIndex, until: i)
            currentWordStartIndex = message.index(after: i)
        }
    }
    
    reverseCharacters(&message, from: currentWordStartIndex, until: message.endIndex)
}


var a = "the eagle has landed"
reverseWords(&a)
print(a)

```

## Merge Sorted Arrays

```swift

// Time: O(n)
// Space - O(n)

let myArray = [3, 4, 6, 10, 11, 15]
let alicesArray = [1, 5, 8, 12, 14, 19]

print(mergeArrays(myArray, alicesArray))

func mergeArrays<Element: Comparable>(_ left: [Element], _ right: [Element]) -> [Element] {
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
    } else {
        result.append(contentsOf: right[rightIndex...])
    }
    
    return result
}

```

## Merge K Sorted Arrays

```swift

// Time: O(kn log k)
// Space: O(kn)

struct Heap<Element: Comparable> {
    let sort: (Element, Element) -> Bool
    var elements: [Element] = []
    
    init(sort: @escaping (Element, Element) -> Bool, elements: [Element] = []) {
        self.sort = sort
        self.elements = elements
        
        if !isEmpty {
            for i in stride(from: count / 2 - 1, through: 0, by: -1) {
                siftDown(from: i)
            }
        }
    }
    
    var count: Int {
        return elements.count
    }
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
}

extension Heap {
    func leftChildIndex(ofParentAt index: Int) -> Int {
        return 2 * index + 1
    }
    
    func rightChildIndex(ofParentAt index: Int) -> Int {
        return 2 * index + 2
    }
    
    func parentIndex(ofChildAt index: Int) -> Int {
        return (index - 1) / 2
    }
}

extension Heap {
    mutating func insert(_ element: Element) {
        elements.append(element)
        siftUp(from: count - 1)
    }
    
    mutating func remove() -> Element? {
        guard !isEmpty else { return nil }
        
        elements.swapAt(0, count - 1)
        
        defer {
            siftDown(from: 0)
        }
        
        return elements.popLast()
    }
}

extension Heap {
    mutating func siftDown(from index: Int) {
        var parent = index
        while true {
            let left = leftChildIndex(ofParentAt: parent)
            let right = rightChildIndex(ofParentAt: parent)
            var candidate = parent
            
            if left < count && sort(elements[left], elements[candidate]) {
                candidate = left
            }
            
            if right < count && sort(elements[right], elements[candidate]) {
                candidate = right
            }
            
            if candidate == parent {
                return
            }
            
            elements.swapAt(parent, candidate)
            parent = candidate
        }
    }
    
    mutating func siftUp(from index: Int) {
        var child = index
        var parent = parentIndex(ofChildAt: child)
        
        while child > 0 && sort(elements[child], elements[parent]) {
            elements.swapAt(parent, child)
            child = parent
            parent = parentIndex(ofChildAt: child)
        }
    }
}

struct PriorityQueue<Element: Comparable> {
    var elements: Heap<Element>
    
    init(sort: @escaping (Element, Element) -> Bool = (<), elements: [Element] = []) {
        self.elements = Heap<Element>(sort: sort, elements: elements)
    }
    
    mutating func enqueue(_ element: Element) {
        elements.insert(element)
    }
    
    mutating func dequeue() -> Element? {
        return elements.remove()
    }
}

struct QueueElement<Element: Comparable> {
    var arrayIndex: Int
    var elementIndex: Int
    var value: Element
}

extension QueueElement: Comparable & Equatable {
    static func < (lhs: QueueElement, rhs: QueueElement) -> Bool {
        return lhs.value < rhs.value
    }
    
    static func == (lhs: QueueElement, rhs: QueueElement) -> Bool {
        return lhs.value == rhs.value
    }
}

func merge<Element: Comparable>(_ arrays: [[Element]]) -> [Element] {
    var priorityQueue = PriorityQueue<QueueElement<Element>>()
    var result = [Element]()
    
    for index in arrays.indices {
        guard !arrays[index].isEmpty else { continue }
        
        let queueElement = QueueElement(arrayIndex: index,
                                        elementIndex: 0,
                                        value: arrays[index][0])
        priorityQueue.enqueue(queueElement)
    }
    
    while let element = priorityQueue.dequeue() {
        result.append(element.value)
        
        var nextIndex = element.elementIndex + 1
        if nextIndex < arrays[element.arrayIndex].count {
            let queueElement = QueueElement(arrayIndex: element.arrayIndex,
                                            elementIndex: nextIndex,
                                            value: arrays[element.arrayIndex][nextIndex])
            priorityQueue.enqueue(queueElement)
        }
        
    }
    return result
}

var a = [
    [1, 2, 3],
    [9, 10, 11, 13, 15],
    [7, 8, 9]
]

print(merge(a))

```

## Single Riffle

```swift
// Time: O(n)
// Space: O(1)

func isSingleRiffle(_ shuffledDeck: [Int], _ half1: [Int], _ half2: [Int]) -> Bool {
    var half1Index = 0
    var half2Index = 0

    for card in shuffledDeck {
        if half1Index < half1.count, card == half1[half1Index] {
            half1Index += 1
            continue
        }
        
        if half2Index < half2.count, card == half2[half2Index] {
            half2Index += 1
            continue
        }
        
        return false
    }
    return true
}

isSingleRiffle(Array(1...51), Array(1...26), Array(27...51))

```

## Inflight Entertainment

```swift
// Time: O(n)
// Space: O(n)

extension Array where Element: Numeric & Hashable  {
    func twoSum(for target: Element) -> Bool {
        var elementSet = Set<Element>()
        
        for element in self {
            let compliment = target - element
            if elementSet.contains(compliment) {
                return true
            }
            elementSet.insert(element)
        }
        return false
    }
}

[90, 91, 120, 180, 123, 154, 122, 164].twoSum(for: 180)
```

## Inflight Entertainment - Guaranteed Sort

```swift
// Time: O(n)
// Space: O(1)

extension Array where Element == Int  {
    func twoSum(for target: Element) -> Bool {
        var left = startIndex
        var right = index(before: endIndex)
        
        while left <= right {
            let sum = self[left] + self[right]
            
            if sum == target {
                return true
            }
            
            if sum > target {
                right = index(before: right)
            } else {
                left = index(after: left)
            }
        }
        return false
    }
}

[90, 91, 120, 123, 154, 164, 180].twoSum(for: 210)

```

## Inflight Entertainment - n movies

```swift
extension Array where Element == Int {
    func nSum(for target: Int) -> Bool {
        guard !isEmpty else { return false }
        guard reduce(0, +) != target else { return true }
        
        for index in indices {
            let left = self[..<index]
            let right = self[(index+1)...]
            let subSet = Array(left + right)
            
            if subSet.nSum(for: target) {
                return true
            }
        }
        return false
    }
}


[1, 3, 9, 2].nSum(for: 5)

```

## Permutation Palindrome

### Optimized Solution

```swift

// Time: O(n)
extension String {
    func permutationPalindrome() -> Bool {
        var unpairedCharacters = Set<Character>()
        
        for character in self {
            if unpairedCharacters.contains(character) {
                unpairedCharacters.remove(character)
            } else {
                unpairedCharacters.insert(character)
            }
        }
        return unpairedCharacters.count <= 1
    }
}

"ivicc".permutationPalindrome()
```

### Fast Solution

```swift

// Time: O(n)
// Space: O(k) where k = Possible unicode character combinations

extension Sequence where Element: Hashable {
    func frequencies() -> [Element: Int] {
        return reduce(into: [:]) {
            $0[$1, default: 0] += 1
        }
    }
}

extension Int {
    var isOdd: Bool {
        return self % 2 != 0
    }
}

extension String {
    func permutationPalindrome() -> Bool {
        return frequencies()
                .filter { (_, value) in value.isOdd }
                .count <= 1
    }
}

"ivicc".permutationPalindrome()

```

### Slow Solution 2

```swift

// Time: O(n!n)

extension String {
    func permutationPalindrome(_ current: String = "") -> Bool {
        guard !isEmpty else {
            return current.isPalindrome
        }
        
        let strArray = Array(self)
        
        for index in strArray.indices {
            let left = String(strArray[0..<index])
            let right = String(strArray[index+1..<count])
            let string = left + right
            
            if string.permutationPalindrome(current + String(strArray[index])) {
                return true
            }
        }
        return false
    }
}

extension String {
    var isPalindrome: Bool {
        var left = startIndex
        var right = index(before: endIndex)
        
        while left < right {
            if self[left] != self[right] {
                return false
            }
            left = index(after: left)
            right = index(before: right)
        }
        return true
    }
}

"ivicc".permutationPalindrome()

```

## Top Scores

```swift
// Time - O(n)
// Space - O(n)

let unsortedScores = [37, 89, 41, 90, 90, 90, 90, 65, 91, 53]
let highestPossibleScore = 100

let sortedScores = sortScores(unsortedScores, withHighest: highestPossibleScore)

func sortScores(_ unsortedScores: [Int], withHighest highestPossibleScore: Int) -> [Int] {
    return unsortedScores.countingSort(with: highestPossibleScore)
}

extension Array where Element == Int {
    func countingSort(with upperBound: Element) -> [Element] {
        guard !isEmpty else { return self }
        
        var buckets: [Element] = .init(repeating: 0, count: upperBound + 1)
        for element in self {
            buckets[element] += 1
        }
        
        for index in 1..<buckets.count {
            let sum = buckets[index] + buckets[index - 1]
            buckets[index] = sum
        }
        
        var sortedArray: [Element] = .init(repeating: 0, count: count)
        
        for element in self {
            buckets[element] -= 1
            sortedArray[buckets[element]] = element
        }
        return sortedArray
    }
}
```

## Apple Stocks

```swift
let stockPrices = [10, 7, 5, 4, 3, 2]

getMaxProfit(from: stockPrices)

func getMaxProfit(from stockPricesYesterday: [Int]) -> Int? {
    guard stockPricesYesterday.count >= 2 else {
        return nil
    }
    
    var minPrice  = stockPricesYesterday[0]
    var maxProfit = stockPricesYesterday[1] - stockPricesYesterday[0]
    
    for currentPrice in stockPricesYesterday.dropFirst() {
        let potentialProfit = currentPrice - minPrice

        maxProfit = max(maxProfit, potentialProfit)
        minPrice = min(minPrice, currentPrice)
    }
    
    return maxProfit
}

```

## Highest Product of 3

```swift
var ints = [1, 10, -5, 1, -100]

extension Array where Element == Int {
    func highestProduct() -> Int {
        var highest = Swift.max(self[0], self[1])
        var lowest = Swift.min(self[0], self[1])
        
        var highest2 = self[0] * self[1]
        var lowest2 = self[0] * self[1]
        
        var highest3 = self[0] * self[1] * self[2]
        
        for number in self {
            highest3 = Swift.max(highest3, highest2 * number, lowest2 * number)
            
            highest2 = Swift.max(highest2, highest * number, lowest * number)
            lowest2 = Swift.min(lowest2, highest * number, lowest * number)
            
            highest = Swift.max(highest, number)
            lowest = Swift.min(lowest, number)
        }
        
        return highest3
    }
}

ints.highestProduct()
```

## Product of All Other Numbers

### Optimized Solution

```swift
var a = [1, 7, 3, 4]

extension Sequence {
    func accumulate<T>(_ initialResult: T, _ nextPartialResult: (T, Element) -> T) -> [T] {
        var result = initialResult
        
        return map {
            let currentResult = result
            result = nextPartialResult(result, $0)
            return currentResult
        }
    }
}

extension Array where Element == Int {
    func products() -> [Element] {
        guard !isEmpty else { return self }
        
        var products = accumulate(1, *)
        var currentProduct = 1
        for i in indices.reversed() {
            products[i] *= currentProduct
            currentProduct *= self[i]
        }
        return products
    }
}

a.products()
```

### Functional Solution

```swift
var a = [1, 7, 3, 4]

extension Sequence {
    func accumulate<T>(_ initialResult: T, _ nextPartialResult: (T, Element) -> T) -> [T] {
        var result = initialResult
        
        return map {
            let currentResult = result
            result = nextPartialResult(result, $0)
            return currentResult
        }
    }
}

extension Array where Element == Int {
    func products() -> [Element] {
        guard !isEmpty else { return self }
        
        let productsBeforeIndex = accumulate(1, *)
        let productsAfterIndex = reversed().accumulate(1, *)
        return zip(productsBeforeIndex, productsAfterIndex.reversed()).reduce(into: []) {
            $0.append($1.0 * $1.1)
        }
    }
}

a.products()

```

## Find Rotation Point

```swift

// Time: O(log n)
// Space: O(1)

var words = [
    "asymptote",  // <-- rotates here!
    "babka",
    "banoffee",
    "engender",
    "karpatka",
    "othellolagkage",
        "ptolemaic",
    "retrograde",
    "supplant",
    "undulate",
    "xenoepist"
]

words.rotationPoint()

extension RandomAccessCollection where Element: Comparable {
    func rotationPoint() -> Index? {
        guard count > 0 else { return nil }
        
        var firstWord = self[startIndex]
        
        var left = startIndex
        var right = index(before: endIndex)

        while left <= right {
            let size = distance(from: left, to: right)
            let middleIndex = index(left, offsetBy: size / 2)
            let middleValue = self[middleIndex]

            if middleValue < firstWord {
                right = index(before: middleIndex)
            } else {
                left = index(after: middleIndex)
            }
        }
        
        let size = distance(from: left, to: right)
        let middleIndex = index(left, offsetBy: size / 2)
        
        return middleIndex == endIndex ? startIndex : middleIndex
    }
}

```


## In-Place Shuffle

```swift

import Foundation

func getRandom(floor: Int, ceiling: Int) -> Int {
    let upperBound = UInt32(ceiling - floor + 1)
    return floor + Int(arc4random_uniform(upperBound))
}

extension MutableCollection where Self: BidirectionalCollection, Index == Int {
    mutating func shuffling() {
        guard !isEmpty else { return }
        
        for currentIndex in indices.dropLast() {
            let randomIndex = getRandom(floor: currentIndex, ceiling: index(before: endIndex))
            swapAt(currentIndex, randomIndex)
        }
    }
}

var a = [1, 2, 3, 4, 5]
a.shuffling()

```
## Largest element in the stack
```swift
struct Stack<Element> {
    var elements: [Element] = []
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
    
    func peek() -> Element? {
        return elements.last
    }
}

struct MaxStack<Element: Comparable> {
    var base = Stack<Element>()
    var maxes = Stack<Element>()
    
    var max: Element? {
        return maxes.peek()
    }
    
    mutating func push(_ element: Element) {
        if let max = maxes.peek() {
            if max <= element {
                maxes.push(element)
            }
        } else {
            maxes.push(element)
        }
        
        base.push(element)
    }
    
    mutating func pop() -> Element? {
        let value = base.pop()
        if let max = maxes.peek(), max == value {
            maxes.pop()
        }
        return value
    }
    
    func peek() -> Element? {
        return base.peek()
    }
}

var maxStack = MaxStack<Int>()
maxStack.push(1)
maxStack.max
maxStack.push(2)
maxStack.max
maxStack.push(10)
maxStack.max
maxStack.push(5)
maxStack.max
maxStack.push(44)
maxStack.max
maxStack.pop()
maxStack.max
maxStack.push(3)
maxStack.max
maxStack.pop()
maxStack.max
maxStack.pop()
maxStack.max
maxStack.pop()
maxStack.max
maxStack.pop()
maxStack.max
maxStack.pop()

```

## Closing Parens

```swift
extension String {
    func closingParenthesis(for index: Index) -> Index? {
        var openParenthesis = 0
        
        for position in self[index...].indices  {
            let character = self[position]
            
            if character == "(" {
                openParenthesis += 1
            }
            
            if character == ")" {
                openParenthesis -= 1
            }
            
            if openParenthesis == 0 {
                return position
            }
        }
        return nil
    }
}

let string = "Sometimes (when I nest them (my parentheticals) too much (like this (and this))) they get confusing."
let index = string.index(string.startIndex, offsetBy: 10)
let closingIndex = string.closingParenthesis(for: index)
string[closingIndex!]

```

## Bracket Validator

```swift
struct Stack<Element> {
    var elements: [Element] = []
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
    
    func peek() -> Element? {
        return elements.last
    }
}

extension String {
    func validBrackets() -> Bool {
        var stack = Stack<Element>()
        let openersToClosers: [Character: Character] = ["(": ")",
                                                        "[": "]",
                                                        "{": "}"]
        
        let openers = Set<Character>(openersToClosers.keys)
        let closers = Set<Character>(openersToClosers.values)
        
        for character in self {
            if openers.contains(character) {
                stack.push(character)
                continue
            }
            
            if closers.contains(character) {
                guard let top = stack.pop() else {
                    return false
                }
                
                if openersToClosers[top] != character {
                    return false
                }
            }
        }
        return true
    }
}

"{ [ ] ( ) }".validBrackets()
"{ [ ( ] ) }".validBrackets()
"{ [ }".validBrackets()


```
