# Tree and Graph Questions

## Route Between Nodes

**Source:** 
* Cracking the Coding Interview - Chapter 4

**Complexity**
* Time: ```O(V + E)```
* Space: ```O(V)```
```swift
extension Graph where Element: Hashable {
    func route(between start: Vertex<Element>, and end: Vertex<Element>) -> Bool {
        var queue = Queue<Vertex<Element>>()
        var enqueued = Set<Vertex<Element>>()
        
        queue.enqueue(start)
        enqueued.insert(start)
        
        while let vertex = queue.dequeue() {
            for edge in edges(from: vertex) {
                guard edge.destination != end else {
                    return true
                }
                
                if !enqueued.contains(edge.destination) {
                    queue.enqueue(edge.destination)
                    enqueued.insert(edge.destination)
                }
            }
        }
        return false
    }
}
```

## Minimal Tree

**Source:** 
* Cracking the Coding Interview - Chapter 4

**Complexity**
* Time: ```O(n)```
* Space: ```O(n)```
```swift
class BinaryNode<Value> {
    let value: Value
    var leftChild: BinaryNode?
    var rightChild: BinaryNode?
    
    init(_ value: Value) {
        self.value = value
    }
}

extension BinaryNode {
    func inOrder() {
        leftChild?.inOrder()
        print(value)
        rightChild?.inOrder()
    }
}

func minBST<CollectionType: RandomAccessCollection>(_ collection: CollectionType) -> BinaryNode<CollectionType.Element>? {
    guard !collection.isEmpty else {
        return nil
    }
    
    let size = collection.distance(from: collection.startIndex, to: collection.endIndex)
    let middleIndex = collection.index(collection.startIndex, offsetBy: size / 2)
    let middleValue = collection[middleIndex]
    
    let root = BinaryNode(middleValue)
    root.leftChild = minBST(collection[..<middleIndex])
    root.rightChild = minBST(collection[collection.index(after: middleIndex)...])
    return root
}

minBST([1, 2, 3, 4, 5, 6, 7, 8, 9])?.inOrder()
```

## List of Depths

**Source:** 
* Cracking the Coding Interview - Chapter 4

**Complexity**
* Time: ```O(n)```
* Space: ```O(n)```
```swift
class Node<Value> {
    let value: Value
    var next: Node? = nil
    
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
    
    mutating func push(_ element: Element) {
        head = Node(element, next: head)
        
        if tail == nil {
            tail = head
        }
    }
    
    mutating func append(_ element: Element) {
        guard !isEmpty else {
            push(element)
            return
        }
        
        tail?.next = Node(element)
        tail = tail?.next
    }
    
    mutating func pop() -> Element? {
        defer {
            head = head?.next
            if isEmpty {
                tail = nil
            }
        }
        return head?.value
    }
}

extension LinkedList: Sequence {
    struct LinkedListIterator: IteratorProtocol {
        var base: LinkedList
        
        mutating func next() -> Element? {
            return base.pop()
        }
    }
    
    func makeIterator() -> LinkedListIterator {
        return LinkedListIterator(base: self)
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

class BinaryNode<Value> {
    let value: Value
    var leftChild: BinaryNode?
    var rightChild: BinaryNode?
    
    init(_ value: Value) {
        self.value = value
    }
}

extension BinaryNode: CustomStringConvertible {
    var description: String {
        return "\(value)"
    }
}

extension BinaryNode {
    func createLevelLinkedList() -> [LinkedList<BinaryNode<Value>>] {
        var result: [LinkedList<BinaryNode<Value>>] = []
        var current = LinkedList<BinaryNode<Value>>()
        current.append(self)
        
        while !current.isEmpty {
            result.append(current)
            var parents = current
            current = LinkedList<BinaryNode<Value>>()
            
            for parent in parents {
                if let left = parent.leftChild {
                    current.append(left)
                }
                
                if let right = parent.rightChild {
                    current.append(right)
                }
            }
        }
        return result
    }
}

let zero  = BinaryNode(0)
let one   = BinaryNode(1)
let five  = BinaryNode(5)
let seven = BinaryNode(7) // Root
let eight = BinaryNode(8)
let nine  = BinaryNode(9)

seven.leftChild  = one
one.leftChild    = zero
one.rightChild   = five
seven.rightChild = nine
nine.leftChild   = eight

print(seven.createLevelLinkedList())
```

## Check Balanced

**Source:** 
* Cracking the Coding Interview - Chapter 4

**Complexity**
* Time: ```O(n)```
* Space: ```O(h)```
```swift
import Foundation

class BinaryNode<Value> {
    let value: Value
    var leftChild: BinaryNode?
    var rightChild: BinaryNode?
    
    init(_ value: Value) {
        self.value = value
    }
}

extension BinaryNode: CustomStringConvertible {
    var description: String {
        return "\(value)"
    }
}

extension BinaryNode {
    func checkHeight() -> Int {
        let leftHeight = leftChild?.checkHeight() ?? -1
        guard leftHeight != Int.min else {
            return Int.min
        }
        
        let rightHeight = rightChild?.checkHeight() ?? -1
        guard rightHeight != Int.min else {
            return Int.min
        }
        
        let heightDiff = leftHeight - rightHeight
        guard abs(heightDiff) <= 1 else {
            return Int.min
        }
        return max(leftHeight, rightHeight) + 1
    }
    
    func isBalanced() -> Bool {
        return checkHeight() != Int.min
    }
}

let zero    = BinaryNode(0)
let one     = BinaryNode(1)
let five    = BinaryNode(5)
let seven   = BinaryNode(7) // Root
let eight   = BinaryNode(8)
let nine    = BinaryNode(9)
let ten     = BinaryNode(10)
let eleven  = BinaryNode(11)
let twelve  = BinaryNode(12)

seven.leftChild  = one
one.leftChild    = zero
one.rightChild   = five
seven.rightChild = nine
nine.leftChild   = eight
eight.leftChild  = ten
ten.leftChild    = eleven
eleven.leftChild = twelve

seven.isBalanced()
```


## Route Between Nodes

**Source:** 
* Cracking the Coding Interview - Chapter 4

**Complexity**
* Time: ```O(n)```
* Space: ```O(n)``` (```O(log n)``` if we can guarantee the tree is balanced)

class BinaryNode<Value> {
    let value: Value
    var leftChild: BinaryNode?
    var rightChild: BinaryNode?
    
    init(_ value: Value) {
        self.value = value
    }
}

extension BinaryNode: CustomStringConvertible {
    var description: String {
        return "\(value)"
    }
}

extension BinaryNode where Value == Int {
    func isBST(_ range: ClosedRange<Int> = Int.min...Int.max) -> Bool {
        guard range.contains(value) else {
            return false
        }
        return leftChild?.isBST(Int.min...value) ?? true && rightChild?.isBST(value...Int.max) ?? true
    }
}

let zero    = BinaryNode(0)
let one     = BinaryNode(1)
let five    = BinaryNode(5)
let seven   = BinaryNode(7) // Root
let eight   = BinaryNode(8)
let nine    = BinaryNode(9)

seven.leftChild  = one
one.leftChild    = zero
one.rightChild   = five
seven.rightChild = nine
nine.leftChild   = eight

seven.isBST()
