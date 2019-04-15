# Stack - With Linked List

```swift
struct Stack<Element> {
    private var elements: LinkedList<Element> = []
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    mutating func push(_ element: Element) {
        elements.push(element)
    }
    
    mutating func pop() -> Element? {
        return elements.pop()
    }
    
    func peek() -> Element? {
        return elements.first
    }
}

extension Stack: ExpressibleByArrayLiteral {
    init(arrayLiteral: Element...) {
        for element in arrayLiteral {
            push(element)
        }
    }
}

extension Stack: CustomStringConvertible {
    var description: String {
        return elements.description
    }
}
```

## Auxiliary Data Structures

### Linked List
```swift
struct LinkedList<Element> {
    var head: Node<Element>?
    
    var isEmpty: Bool {
        return head == nil
    }
    
    var first: Element? {
        return head?.value
    }
    
    mutating func push(_ element: Element) {
        head = Node(element, next: head)
    }
    
    mutating func pop() -> Element? {
        defer {
            head = head?.next
        }
        return head?.value
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
```

### Linked List Node
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
