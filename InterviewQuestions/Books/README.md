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

## sum lists forward

```swift
import Foundation

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

extension Node {
    var count: Int {
        var count = 0
        var current: Node? = self
        
        while current != nil {
            current = current?.next
            count += 1
        }
        
        return count
    }
}

extension Node where Value == Int {
    func padZeros(_ count: Int) -> Node {
        guard count > 0 else {
            return self
        }
        
        var head = self
        
        for _ in 0..<count {
            head = Node(0, next: head)
        }
        
        return head
    }
}

var node1 = Node(5)
var node2 = Node(6)
var node3 = Node(3)
var node8 = Node(9)
var node9 = Node(8)


node1.next = node2
node2.next = node3
node3.next = node8
node8.next = node9

var node4 = Node(8)
var node5 = Node(4)
var node6 = Node(2)

node4.next = node5
node5.next = node6

func sumForward(_ list1: Node<Int>, _ list2: Node<Int>) -> Node<Int> {
    var list1 = list1
    var list2 = list2
    
    let list1Count = list1.count
    let list2Count = list2.count
    
    let difference = abs(list1Count - list2Count)
    
    if list1Count < list2Count {
        list1 = list1.padZeros(difference)
    } else {
        list2 = list2.padZeros(difference)
    }
    
    var (head, carry) = addValues(list1, list2)
    
    if carry > 0 {
        head = Node(carry, next: head)
    }
    
    return head!
    
}

func addValues(_ list1: Node<Int>?, _ list2: Node<Int>?) -> (node: Node<Int>?, carry: Int) {
    guard let list1 = list1, let list2 = list2 else {
        return (nil, 0)
    }
    
    let (nextNode, nextCarry) = addValues(list1.next, list2.next)
    let total = nextCarry + list1.value + list2.value
    let value = total % 10
    let carry = total / 10
    let node = Node(value, next: nextNode)
    return (node, carry)
}

sumForward(node1, node4)

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

struct MinStack {
    var base = Stack<Int>()
    var minElement: Int?
    
    mutating func push(_ element: Int) {
        if minElement == nil {
            minElement = element
        }

        guard element < minElement! else {
            base.push(element)
            return
        }

        let value = 2 * element - minElement!
        minElement = element
        base.push(value)
    }
    
    mutating func pop() -> Int? {
        guard let value = base.pop() else {
            minElement = nil
            return nil
        }
        
        guard let min = minElement else {
            return nil
        }
        
        guard value >= min else {
            defer {
                minElement = 2 * min - value
            }
            return minElement
        }
        
        return value
    }
    
    var min: Int? {
        return minElement
    }
}

var stack = MinStack()
stack.push(5)
stack.min
stack.push(4)
stack.min
stack.push(6)
stack.min
stack.push(1)
stack.min
stack.push(3)
stack.min

print(stack.base)

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

struct MaxStack {
    var base = Stack<Int>()
    var maxElement: Int?
    
    mutating func push(_ element: Int) {
        if maxElement == nil {
            maxElement = element
        }

        guard element > maxElement! else {
            base.push(element)
            return
        }

        let value = 2 * element - maxElement!
        maxElement = element
        base.push(value)
    }
    
    mutating func pop() -> Int? {
        guard let value = base.pop() else {
            minElement = nil
            return nil
        }
        
        guard let max = maxElement else {
            return nil
        }
        
        guard value <= max else {
            defer {
                maxElement = 2 * max - value
            }
            return maxElement
        }
        
        return value
    }
    
    var max: Int? {
        return maxElement
    }
}

var stack = MaxStack()
stack.push(5)
stack.max
stack.push(4)
stack.max
stack.push(6)
stack.max
stack.push(1)
stack.max
stack.push(3)
stack.max

print(stack.base)

stack.pop()
stack.max
stack.pop()
stack.max
stack.pop()
stack.max
stack.pop()
stack.max
stack.pop()
stack.max


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

## Flatten a linked list

```swift
class Node<Value> {
    let value: Value
    var next: Node?
    var child: Node?
    
    init(_ value: Value, next: Node? = nil, child: Node? = nil) {
        self.value = value
        self.next = next
        self.child = child
    }
}

struct Stack<Element> {
    var elements = [Element]()
    
    mutating func push(_ element: Element) {
        return elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
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

extension Node {
    func flatten() {
        var tail: Node? = self
        while tail?.next != nil {
            tail = tail?.next
        }
        
        var current: Node? = self
        while current != nil {
            if current?.child != nil {
                tail?.next = current?.child
                current?.child = nil
                
                while tail?.next != nil {
                    tail = tail?.next
                }
            }
            current = current?.next
        }
    }
    
    func flatten2() {
        var current: Node? = self
        var nextLinks = Stack<Node?>()
        
        while current != nil {
            if current?.child != nil {
                if current?.next != nil {
                    nextLinks.push(current?.next)
                }
                current?.next = current?.child
                current?.child = nil
            }

            if current?.next == nil {
                current?.next = nextLinks.pop() ?? nil
            }

            current = current?.next
        }
    }
}




var node10 = Node(10)
var node5  = Node(5)
var node12 = Node(12)
var node7  = Node(7)
var node11 = Node(11)
var node4  = Node(4)
var node20 = Node(20)
var node13 = Node(13)
var node17 = Node(17)
var node6  = Node(6)
var node2  = Node(2)
var node16 = Node(16)
var node9  = Node(9)
var node8  = Node(8)
var node3  = Node(3)
var node19 = Node(19)
var node15 = Node(15)

node10.next = node5
node5.next = node12
node12.next = node7
node7.next = node11

node10.child = node4
node4.next = node20
node20.child = node2
node20.next = node13
node13.child = node16
node16.child = node3

node7.child = node17
node17.next = node6
node17.child = node9
node9.next = node8
node9.child = node19
node19.next = node15

node10.flatten2()
print(node10)

```


## Find cycle in a linked list

```swift
class Node<Value> {
    let value: Value
    var next: Node?
    
    init(_ value: Value, next: Node? = nil) {
        self.value = value
        self.next = next
    }
}

extension Node: Equatable where Value: Equatable {
    static func == (lhs: Node, rhs: Node) -> Bool {
        return lhs.value == rhs.value
    }
}

extension Node: Hashable where Value: Hashable {
    var hashValue: Int {
        return value.hashValue
    }
}

extension Node {
    var isCyclic: Bool {
        var fast: Node? = self
        var slow: Node? = self
        
        while fast != nil && fast?.next != nil {
            fast = fast?.next?.next
            slow = slow?.next
            
            if fast === slow {
                return true
            }
        }
        return false
    }
}

}

import Foundation

class Node<Value> {
    let value: Value
    var next: Node?
    
    init(_ value: Value, next: Node? = nil) {
        self.value = value
        self.next = next
    }
}

extension Node: Equatable {
        static func == (lhs: Node, rhs: Node) -> Bool {
        return lhs === rhs
    }
}

extension Node: Hashable where Value: Hashable {
    func hash(into hasher: inout Hasher) {
        hasher.combine(ObjectIdentifier(self))
    }
}

extension Node {
    var isCyclic: Bool {
        var fast: Node? = self
        var slow: Node? = self
        
        while fast != nil && fast?.next != nil {
            fast = fast?.next?.next
            slow = slow?.next

            if fast === slow {
                return true
            }
        }
        return false
    }
}

https://ericasadun.com/2017/06/01/using-memory-addresses-for-hashing/
https://www.youtube.com/watch?v=SWWK9e7xUxw&feature=youtu.be
extension Node where Value: Hashable {
    var isCyclic2: Bool {
        var seenNodes = Set<Node>()
        var current: Node? = self
        
        while current != nil {
            if seenNodes.contains(current!) {
                return true
            }
            seenNodes.insert(current!)
            current = current?.next
        }
        return false
    }
}
protocol PointerHashable: class, Hashable {}

extension PointerHashable {
    static func == (left: Self, right: Self) -> Bool {
        return left === right
    }
    
    var hashValue: Int {
        return ObjectIdentifier(self).hashValue
    }
}

var node1 = Node(1)
var node2 = Node(2)
var node3 = Node(3)
var node4 = Node(4)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node1

node1.isCyclic2

ObjectIdentifier(node1)
ObjectIdentifier(node2)
ObjectIdentifier(node1)
ObjectIdentifier(node1)


var node1 = Node(1)
var node2 = Node(2)
var node3 = Node(3)
var node4 = Node(4)
var node5 = Node(5)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node2

node1.isCyclic2

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

## unflatten linkedlist

```swift
class Node<Value> {
    let value: Value
    var next: Node?
    var child: Node?
    var previous: Node?
    
    init(_ value: Value, next: Node? = nil, child: Node? = nil, previous: Node? = nil) {
        self.value = value
        self.next = next
        self.child = child
        self.previous = previous
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

extension Node {
    func flatten() {
        var tail: Node? = self
        while tail?.next != nil {
            tail = tail?.next
        }
        
        var current: Node? = self
        while current != nil {
            if current?.child != nil {
                tail?.next = current?.child
                current?.child?.previous = tail
                
                while tail?.next != nil {
                    tail = tail?.next
                }
            }
            current = current?.next
        }
    }
    
    
    func unflatten() {
        var current: Node? = self
        
        while current != nil {
            if let child = current?.child {
                let previousNode = child.previous
                child.previous = nil
                previousNode?.next = nil
                
                child.unflatten()
            }
            current = current?.next
        }
    }

    func t() {
        var current: Node? = self
        while current != nil {
            print(current!.value)
            current?.child?.t()
            current = current?.next
        }
        
    }
}

var node10 = Node(10)
var node5  = Node(5)
var node12 = Node(12)
var node7  = Node(7)
var node11 = Node(11)
var node4  = Node(4)
var node20 = Node(20)
var node13 = Node(13)
var node17 = Node(17)
var node6  = Node(6)
var node2  = Node(2)
var node16 = Node(16)
var node9  = Node(9)
var node8  = Node(8)
var node3  = Node(3)
var node19 = Node(19)
var node15 = Node(15)

node10.next = node5
node5.previous = node10

node5.next = node12
node12.previous = node5

node12.next = node7
node7.previous = node12

node7.next = node11
node11.previous = node7

node10.child = node4
node4.next = node20
node20.previous = node4
node20.child = node2
node20.next = node13
node13.previous = node20
node13.child = node16
node16.child = node3

node7.child = node17
node17.next = node6
node6.previous = node17
node17.child = node9
node9.next = node8
node8.previous = node9
node9.child = node19
node19.next = node15
node15.previous = node19

node10.flatten()

node10.unflatten()
node10.t()

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