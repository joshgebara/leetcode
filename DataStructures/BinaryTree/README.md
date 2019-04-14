## Binary Tree

```swift
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
```
