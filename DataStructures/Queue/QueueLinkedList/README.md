# Queue - With Linked List

```swift
struct Queue<Element> {
    var elements: LinkedList<Element> = []

    var isEmpty: Bool {
        return elements.isEmpty
    }

    mutating func enqueue(_ element: Element) {
        elements.append(element)
    }

    mutating func dequeue() -> Element? {
        return elements.pop()
    }

    func peek() -> Element? {
        return elements.first
    }
}

extension Queue: CustomStringConvertible {
    var description: String {
        return elements.description
    }
}

extension Queue: ExpressibleByArrayLiteral {
    init(arrayLiteral: Element...) {
        for element in arrayLiteral {
            enqueue(element)
        }
    }
}
```

## Auxiliary Data Structures

### Linked List
```swift
struct LinkedList<Element> {
    var head: Node<Element>?
    var tail: Node<Element>?

    var isEmpty: Bool {
        return head == nil
    }

    var first: Element? {
        return head?.value
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
