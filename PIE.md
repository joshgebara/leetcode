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

extension Node where Value: Hashable {
    var isCyclic2: Bool {
        var current: Node? = self
        var visitedNodes = Set<Node>()
        
        while current != nil && current?.next != nil {
            if visitedNodes.contains(current!) {
                return true
            }
            
            visitedNodes.insert(current!)
            current = current?.next
        }
        return false
    }
}

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