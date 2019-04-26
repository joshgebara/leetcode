# Linked List Questions

## Remove Dups

**Source:** 
* Cracking the Coding Interview - Chapter 2

### Using temporary buffer

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

extension Node where Value: Hashable {
    func removeDuplicates() {
        var seenValues = Set<Value>()
        var current: Node? = self
        var previous: Node? = self
        
        while current != nil {
            if seenValues.contains(current!.value) {
                previous?.next = current?.next
            } else {
                seenValues.insert(current!.value)
                previous = current
            }
            current = current?.next
        }
    }
}

var node1 = Node(2)
var node2 = Node(1)
var node3 = Node(1)
var node4 = Node(1)
var node5 = Node(1)
var node6 = Node(2)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6

node1.removeDuplicates()
```

### Without temporary buffer

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

extension Node where Value: Hashable {
    func removeDuplicates() {
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

var node1 = Node(2)
var node2 = Node(1)
var node3 = Node(1)
var node4 = Node(1)
var node5 = Node(1)
var node6 = Node(2)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6

node1.removeDuplicates()
```

## Return Kth to Last

**Source:** 
* Cracking the Coding Interview - Chapter 2
* Interview Cake - Chapter 8
* Programming Interviews Exposed - Chapter 4

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

node1.kthFromEnd(3) // 2
node1.kthFromEnd(-1) // nil
```

## Delete Middle Node

**Source:** 
* Cracking the Coding Interview - Chapter 2
* Interview Cake - Chapter 8

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

## Partition

**Source:** 
* Cracking the Coding Interview - Chapter 2

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

node1.partition(5)?.printNodes() // 1 -> 2 -> 3 -> 5 -> 8 -> 5 -> 10
```

## Sum Lists

**Source:** 
* Cracking the Coding Interview - Chapter 2

### Reverse Order Linked Lists

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

### Forward Order Linked Lists

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

## Palindrome

**Source:** 
* Cracking the Coding Interview - Chapter 2

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
    var isPalindrome: Bool {
        var nodeBeforeMiddle = self.nodeBeforeMiddle()
        var middleNode = nodeBeforeMiddle?.next
        
        nodeBeforeMiddle?.next = nil
        
        var reversedMiddleNode = middleNode?.reverseList()
        let result = nodesEqual(with: reversedMiddleNode)
        
        middleNode = reversedMiddleNode?.reverseList()
        nodeBeforeMiddle?.next = middleNode
        
        return result
    }
    
    func nodesEqual(with other: Node?) -> Bool {
        var list1: Node? = self
        var list2: Node? = other
        var differences = 0

        while list1 != nil || list2 != nil {
            guard differences < 1 else {
                return false
            }

            if list1?.value != list2?.value {
                differences += 1
            }
            
            list1 = list1?.next
            list2 = list2?.next
        }
        return true
    }
}

extension Node {
    var count: Int {
        var current: Node? = self
        var count = 0
        
        while current != nil {
            current = current?.next
            count += 1
        }
        return count
    }
    
    func nodeBeforeMiddle() -> Node? {
        var fast: Node? = self
        var slow: Node? = self
        
        let halfCount = count / 2
        for _ in 0..<halfCount {
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

    func reverseList() -> Node? {
        var current: Node? = self
        var previous: Node? = nil
        var next: Node? = nil
        
        while current != nil {
            next = current?.next
            current?.next = previous
            previous = current
            current = next
        }
        return previous
    }
}

node1.isPalindrome
node1
```

## Intersection

**Source:** 
* Cracking the Coding Interview - Chapter 2

```swift
```

## Loop Detection

**Source:** 
* Cracking the Coding Interview - Chapter 2
* Interview Cake - Chapter 8
* Programming Interviews Exposed - Chapter 4

### Return Bool - O(1) Space
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

var node1 = Node(1)
var node2 = Node(2)
var node3 = Node(3)
var node4 = Node(4)
var node5 = Node(5)
var node6 = Node(6)
var node7 = Node(7)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7
node7.next = node4

node1.isCyclic // true
```

### Return Bool - O(n) Space

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
    var isCyclic: Bool {
        var current: Node? = self
        var seen = Set<Node>()
        
        while let node = current?.next {
            guard !seen.contains(node) else {
                return true
            }
            
            current = node.next
            seen.insert(node)
        }
        return false
    }
}

extension Node: PointerHashable {}

protocol PointerHashable: class, Hashable {}

extension PointerHashable {
    static func == (lhs: Self, rhs: Self) -> Bool {
        return lhs === rhs
    }
    
    func hash(into hasher: inout Hasher) {
        hasher.combine(ObjectIdentifier(self))
    }
}

var node1 = Node(1)
var node2 = Node(2)
var node3 = Node(3)
var node4 = Node(4)
var node5 = Node(5)
var node6 = Node(6)
var node7 = Node(7)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7
node7.next = node4

node1.isCyclic // true
```

### Return Node at Beginning of the Loop

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
        return "\(value)"
    }
}

extension Node {
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
        return "\(value)"
    }
}

extension Node {
    func cycleStart() -> Node? {
        var fast: Node? = self
        var slow: Node? = self
        
        while fast != nil && fast?.next != nil {
            fast = fast?.next?.next
            slow = slow?.next
            
            if fast === slow {
                break
            }
        }
        
        guard fast != nil || fast?.next != nil else {
            return nil
        }
        
        fast = self
        
        while fast !== slow {
            fast = fast?.next
            slow = slow?.next
        }
        
        return fast
    }
}

var node1 = Node(1)
var node2 = Node(2)
var node3 = Node(3)
var node4 = Node(4)
var node5 = Node(5)
var node6 = Node(6)
var node7 = Node(7)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7
node7.next = node4

node1.cycleStart() // 4
```

## Reverse a Linked List

**Source:** 
* Interview Cake - Chapter 8

### Iterative

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
    func reverseIterative() -> Node? {
        var current: Node? = self
        var previous: Node? = nil
        var next: Node? = nil

        while current != nil {
            next = current?.next
            current?.next = previous
            previous = current
            current = next
        }
        
        return previous
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

node1.next = node2
node2.next = node3
node3.next = node4

node1.reverseIterative() // 4 -> 3 -> 2 -> 1
```

### Recursive

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
    func reverseRecursive() -> Node? {
        guard next != nil else { return self }
        
        let head = next?.reverseRecursive()
        next?.next = self
        next = nil
        return head
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

node1.next = node2
node2.next = node3
node3.next = node4

node1.reverseRecursive() // 4 -> 3 -> 2 -> 1
```

## Stack Implementation

**Source:** 
* Programming Interviews Exposed - Chapter 4

```swift

```

## Queue Implementation

**Source:** 
* Data Structures & Algorithms in Swift

```swift

```

## Multilevel List

**Source:** 
* Programming Interviews Exposed - Chapter 4

### Flatten the List Level First
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
    func flattenLevelFirst() {
        var current: Node? = self
        var tail: Node? = self
        
        while tail?.next != nil {
            tail = tail?.next
        }

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
}

var node10 = Node(10) // head
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

node10.flattenLevelFirst() // 10 -> 5 -> 12 -> 7 -> 11 -> 4 -> 20 -> 13 -> 17 -> 6 -> 2 -> 16 -> 9 -> 8 -> 3 -> 19 -> 15
```

### Flatten the List Depth First
```swift
struct Stack<Element> {
    var elements: [Element] = []
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}

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
    func flattenDepthFirst() {
        var stack = Stack<Node>()
        var current: Node? = self
        
        while let next = current?.next {
            if let child = current?.child {
                stack.push(next)
                current?.next = child
                child.previous = current
            }
            current = current?.next
            
            if current?.next == nil {
                current?.next = stack.pop()
            }
        }
    }
}

var node10 = Node(10) // head
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

node10.flattenDepthFirst() // 10 -> 4 -> 20 -> 2 -> 13 -> 16 -> 3 -> 5 -> 12 -> 7 -> 17 -> 9 -> 19 -> 15 -> 8 -> 6 -> 11
```


### Unflatten the List
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
    func flattenLevelFirst() {
        var current: Node? = self
        var tail: Node? = self
        
        while tail?.next != nil {
            tail = tail?.next
        }

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
                let previous = child.previous
                child.previous = nil
                previous?.next = nil
                
                child.unflatten()
            }
            current = current?.next
        }
    }
}

var node10 = Node(10) // head
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

node10.flattenLevelFirst() // 10 -> 5 -> 12 -> 7 -> 11 -> 4 -> 20 -> 13 -> 17 -> 6 -> 2 -> 16 -> 9 -> 8 -> 3 -> 19 -> 15
node10.unflatten() // 10 -> 5 -> 12 -> 7 -> 11
```

## Implement a Singly Linked List
* ```push()```
* ```append()```
* ```insert(after:)```
* ```pop()```
* ```removeLast()```
* ```remove(after:)```
* ```node(at:)```

**Source:** 
* Data Structures & Algorithms in Swift

```swift

```
