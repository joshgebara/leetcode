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