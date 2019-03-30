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

var node4 = Node(2)
var node5 = Node(9)
var node6 = Node(0)
var node7 = Node(9)

node1.next = node2
node2.next = node3

node4.next = node5
node5.next = node6
node6.next = node7

extension Node where Value == Int {
    // Reversed
    func sum(with other: Node) -> Node? {
        let dummyNode: Node? = Node(-1)
        var current = dummyNode
        
        var num1: Node? = self
        var num2: Node? = other
        
        var carry = 0
        
        while let num1Value = num1?.value, let num2Value = num2?.value {
            let total = num1Value + num2Value + carry
            carry = total / 10
            current?.next = Node(total % 10)
            current = current?.next
            num1 = num1?.next
            num2 = num2?.next
        }
        
        if num1 != nil {
            current?.next = num1
        } else {
            current?.next = num2
        }
        
        return dummyNode?.next
    }
}

print(node1.sum(with: node4))

```