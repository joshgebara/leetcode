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