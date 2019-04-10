# Linked Lists

## Remove Dups

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

var node1 = Node(1)
var node2 = Node(1)
var node3 = Node(2)
var node4 = Node(6)
var node5 = Node(4)
var node6 = Node(1)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6

extension Node where Value: Hashable {
    // Optimize for time
    func removeDuplicates() {
        var current: Node? = self
        var previous: Node? = nil
        var seenValues = Set<Value?>()
        
        while current != nil {
            if seenValues.contains(current?.value) {
                previous?.next = current?.next
            } else {
                seenValues.insert(current?.value)
                previous = current
            }
            current = current?.next
        }
    }
    
    // Optimize for space
    func removeDuplicates2() {
        var current: Node? = self
        
        while current != nil {
            var runner = current
            
            while runner?.next != nil {
                if current?.value == runner?.next?.value {
                    runner?.next = runner?.next?.next
                } else {
                    runner = runner?.next
                }
            }
            current = current?.next
        }
    }
}

node1.removeDuplicates2()
print(node1)

```

## Return Kth to Last

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

var node1 = Node(1)
var node2 = Node(1)
var node3 = Node(2)
var node4 = Node(6)
var node5 = Node(4)
var node6 = Node(1)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6

extension Node {
    func kthFromEnd(_ k: Int) -> Node? {
        guard k >= 0 else { return nil }
    
        var fast: Node? = self
        var slow: Node? = self
        
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

node1.kthFromEnd(-1)

```

## Delete Middle Node

```swift
class Node<Value> {
    var value: Value
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

var node1 = Node(1)
var node2 = Node(2)
var node3 = Node(3)
var node4 = Node(4)
var node5 = Node(5)
var node6 = Node(6)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6

extension Node {
    # Note - this problem cannot be solved if the node to be deleted is the last node. This is because we aren't given a reference to the head only the node to be deleted.
    func deleteNode() {
        var current = self
        guard let nextNode = next else {
            return
        }
        
        value = nextNode.value
        current.next = nextNode.next
        nextNode.next = nil
    }
}

node2.deleteNode()
print(node1)

```


## Sum Lists

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

var node1 = Node(3)
var node2 = Node(2)
var node3 = Node(8)

var node4 = Node(9)
var node5 = Node(9)
var node6 = Node(9)
var node7 = Node(9)
var node8 = Node(9)

node1.next = node2
node2.next = node3

node4.next = node5
node5.next = node6
node6.next = node7
node7.next = node8

extension Node where Value == Int {
    // Reversed
    func sum(with other: Node) -> Node? {
        let result: Node? = Node(-1)
        var current = result
        
        var num1: Node? = self
        var num2: Node? = other
        
        var carry = 0
        
        while num1 != nil || num2 != nil {
            let num1Value = num1?.value ?? 0
            let num2Value = num2?.value ?? 0
            
            let total = num1Value + num2Value + carry
            carry = total / 10
            
            current?.next = Node(total % 10)
            current = current?.next
            
            num1 = num1?.next
            num2 = num2?.next
        }
        
        if carry > 0 {
            current?.next = Node(carry)
        }
        
        return result?.next
    }
}

print(node1.sum(with: node4))


```

## Partition

```swift
class Node<Value> {
    let value: Value
    var next: Node?
    
    init(_ value: Value, next: Node? = nil) {
        self.value = value
        self.next = next
    }
}

extension Node {
    func printNodes() {
        var current: Node? = self
        
        while current != nil {
            print(current!, terminator:" -> ")
            current = current?.next
        }
    }
}

extension Node: CustomStringConvertible {
    var description: String {
        return "\(value)"
    }
}

var node1 = Node(3)
var node2 = Node(5)
var node3 = Node(8)
var node4 = Node(5)
var node5 = Node(10)
var node6 = Node(2)
var node7 = Node(1)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7



extension Node where Value: Comparable {
    func partition(_ part: Value) -> Node? {
        var current: Node? = self
        var head: Node? = self
        var tail: Node? = self
        
        while current != nil {
            var next = current?.next
            
            if current!.value < part {
                current?.next = head
                head = current
            } else {
                tail?.next = current
                tail = current
            }
            current = next
        }
        tail?.next = nil
        return head
    }
}

node1.printNodes()
print("\n")
node1.partition(5)!.printNodes()

```

## Palindrome

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

var node1 = Node("R")
var node2 = Node("A")
var node3 = Node("C")
var node4 = Node("E")
var node5 = Node("C")
var node6 = Node("A")
var node7 = Node("R")

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7

struct Stack<Element> {
    var elements: [Element] = []
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
}

extension Node where Value: Equatable {
    func isPalindrome() -> Bool {
        var fast: Node? = self
        var slow: Node? = self
        var stack = Stack<Value>()
        
        while fast != nil && fast?.next != nil {
            stack.push(slow!.value)
            fast = fast?.next?.next
            slow = slow?.next
        }
        
        if fast?.next == nil {
            slow = slow?.next
        }
        
        while slow != nil, let top = stack.pop() {
            if slow!.value != top {
                return false
            }
            slow = slow?.next
        }
        return stack.isEmpty
    }
}

node1.isPalindrome()


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

var node1 = Node("R")
var node2 = Node("A")
var node3 = Node("C")
var node4 = Node("E")
var node5 = Node("C")
var node6 = Node("A")
var node7 = Node("R")

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7

extension Node where Value: Equatable {
    func isPalindrome() -> Bool {
        var fast: Node? = self
        var slow: Node? = self
        var previousNode: Node? = nil
        var middleNode: Node? = nil
        
        while fast != nil && fast?.next != nil {
            previousNode = slow
            fast = fast?.next?.next
            slow = slow?.next
        }
        
        if fast?.next == nil {
            middleNode = slow
            slow = slow?.next
        }
        
        var secondHalf = slow
        previousNode?.next = nil
        secondHalf = secondHalf!.reverse()
        
        let result = self.compare(with: secondHalf)
        
        secondHalf = secondHalf?.reverse()
        
        if middleNode != nil {
            previousNode?.next = middleNode
            middleNode?.next = secondHalf
        } else {
            previousNode?.next = secondHalf
        }
        return result
    }
    
    func compare(with node: Node?) -> Bool {
        var list1: Node? = self
        var list2: Node? = node
        
        while list1 != nil && list2 != nil {
            if list1!.value != list2!.value {
                return false
            }
            list1 = list1?.next
            list2 = list2?.next
        }
        
        if list1 != nil || list2 != nil {
            return false
        }
        
        return true
    }
    
    func reverse() -> Node {
        var current: Node? = self
        var previous: Node? = nil
        var next: Node? = nil
        
        while current != nil {
            next = current?.next
            current?.next = previous
            previous = current
            current = next
        }
        return previous!
    }
}

node1.isPalindrome()

```

## Intersection of two linked lists

```swift
import Darwin

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

var node1 = Node(1)
var node2 = Node(2)
var node3 = Node(3)

var node4 = Node(4)

var node5 = Node(5)
var node6 = Node(6)
var node7 = Node(7)
var node8 = Node(8)
var node9 = Node(9)

node1.next = node2
node2.next = node3
node3.next = node5

node4.next = node5
node5.next = node6
node6.next = node7

extension Node {
    func intersection(with other: Node?) -> Node? {
        var list1: Node? = self
        var list2: Node? = other
        
        let list1Count = list1!.count
        let list2Count = list2!.count
        
        let difference = abs(list1Count - list2Count)
        
        if list1Count > list2Count {
            list1 = list1?.traverse(difference)
        } else {
            list2 = list2?.traverse(difference)
        }
 
        while list1 != nil && list2 != nil {
            if list1 === list2 {
                return list2
            }
            list1 = list1?.next
            list2 = list2?.next
        }
        return nil
    }
    
    func traverse(_ steps: Int) -> Node? {
        var current: Node? = self
        
        for _ in 0..<steps {
            current = current?.next
        }
        return current
    }
    
    var count: Int {
        var current: Node? = self
        var count = 0
        
        while current != nil {
            count += 1
            current = current?.next
        }
        return count
    }
}

print(node1.intersection(with: node4)!.value)

```




//import Darwin
//
//class Node<Value> {
//    let value: Value
//    var next: Node?
//    
//    init(_ value: Value, next: Node? = nil) {
//        self.value = value
//        self.next = next
//    }
//}
//
//extension Node: CustomStringConvertible {
//    var description: String {
//        guard let next = next else {
//            return "\(value)"
//        }
//        return "\(value) -> \(next)"
//    }
//}
//
//var node1 = Node(6)
//var node2 = Node(1)
//var node3 = Node(7)
//
//var node4 = Node(2)
//var node5 = Node(9)
//var node6 = Node(5)
//
//node1.next = node2
//node2.next = node3
//
//node4.next = node5
//node5.next = node6
//
//extension Node where Value == Int {
//    func sumForward(with other: Node) -> Node? {
//        var list1: Node = self
//        var list2: Node = other
//        
//        let list1Length = length()
//        let list2Length = other.length()
//        
//        let difference = abs(list1Length - list2Length)
//        
//        if list1Length > list2Length {
//            list1 = list1.padZeros(difference)
//        } else {
//            list2 = list2.padZeros(difference)
//        }
//        
//        let result = list1.addList(with: list2)
//        
//        if result?.carry == 0 {
//            return result.sum
//        } else {
//            let node = result.sum?.push(result.carry)
//            return node
//        }
//    }
//    
//    func push(_ value: Value) -> Node {
//        let node = Node(value)
//        node.next = self
//        return node
//    }
//    
//    func addList(with other: Node?) -> (sum: Node, carry: Int)? {
//        guard let other = other else {
//            return nil
//        }
//        
//        
//        
////        let sum = next?.addList(with: other.next)
////        print(value)
////        print(other.value)
////        print(sum.carry)
////
////        let total = sum.carry + value + other.value
////        print(total)
////        if sum.sum == nil {
////            let result = Node(total % 10)
////            return (result, total / 10)
////        } else {
////            let result = sum.sum!.push(total % 10)
////            return (result, total / 10)
////        }
//    }
//    
//    func length() -> Int {
//        var count = 0
//        var current: Node? = self
//        
//        while current != nil {
//            count += 1
//            current = current?.next
//        }
//        return count
//    }
//    
//    func padZeros(_ count: Int) -> Node {
//        var head: Node = self
//        
//        for _ in 0..<count {
//            let paddingNode = Node(0)
//            paddingNode.next = head
//            head = paddingNode
//        }
//        return head
//    }
//}
//
//node1.sumForward(with: node4)
////print(node1.sumForward(with: node4)!)



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

## Stack Min

```swift
struct Stack<Element: Comparable> {
    var elements: [Element] = []
    var mins: [Element] = []
    
    var min: Element? {
        return mins.last
    }
    
    mutating func push(_ element: Element) {
        if let min = mins.last {
            if element <= min {
                mins.append(element)
            }
        } else {
            mins.append(element)
        }
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        let value = elements.popLast()
        if let min = mins.last {
            if value == min {
                mins.popLast()
            }
        }
        return value
    }
}


var stack = Stack<Int>()
stack.push(3)
stack.push(4)
stack.push(2)
stack.push(8)
stack.push(3)
stack.push(5)
stack.push(1)
stack.mins
stack.min
stack.pop()
stack.mins
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
stack.pop()
stack.min

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