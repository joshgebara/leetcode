# Linked List

```swift
struct LinkedList<Element> {
    var head: Node<Element>?
    var tail: Node<Element>?
}

extension LinkedList {
    var isEmpty: Bool {
        return head == nil
    }
    
    var count: Int {
        var current = head
        var count = 0
        
        while current != nil {
            current = current?.next
            count += 1
        }
        
        return count
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
}

extension LinkedList {
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
    
    mutating func insert(_ element: Element, after node: Node<Element>) {
        guard node !== tail else {
            append(element)
            return
        }
        node.next = Node(element, next: node.next)
    }
}

extension LinkedList {
    mutating func pop() -> Element? {
        defer {
            head = head?.next
            if isEmpty {
                tail = nil
            }
        }
        return head?.value
    }
    
    mutating func removeLast() -> Element? {
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
    
    mutating func remove(after node: Node<Element>) -> Element? {
        defer {
            if node.next === tail {
                tail = node
            }
            node.next = node.next?.next
        }
        return node.next?.value
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

extension LinkedList: ExpressibleByArrayLiteral {
    init(arrayLiteral: Element...) {
        for element in arrayLiteral {
            append(element)
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
```

# Linked List Node
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
```
