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
    
    mutating func last(_ place: Int) -> Node<Element>? {
        var fast = head
        var slow = head
        
        for _ in 0..<place {
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
    
    init(_ value: Value, next: Node? = nil, child: Node? = nil) {
        self.value = value
        self.next = next
        self.child = child
    }
}

extension Node: CustomStringConvertible {
    var description: String {
        if next != nil && child != nil {
            return "\(value) \n \(child!) \n -> \(next!)"
        }
        
        if next != nil {
            return "\(value) -> \(next!)"
        }
        
        if child != nil {
            return "\(value) \n \(child!)"
        }
        
        return "\(value)"
    }
}

extension Node {
    func flatten() {
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
                var nextLink: Node? = nextLinks.pop() ?? nil
                current?.next = nextLink
                nextLink = nextLink?.next
            }

            current = current?.next
        }
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
var node10 = Node(10)

node1.next = node5
node1.child = node2
node2.next = node3
node2.child = node10
node3.child = node4
node5.next = node9
node5.child = node6
node6.child = node7
node7.child = node8

node1.flatten()
print(node1)

```