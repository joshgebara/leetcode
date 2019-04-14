# Tree

```swift
class TreeNode<Value> {
    let value: Value
    var children: [TreeNode] = []
    
    init(_ value: Value) {
        self.value = value
    }
    
    func add(_ child: TreeNode) {
        children.append(child)
    }
}

extension TreeNode: CustomStringConvertible {
    var description: String {
        return "\(value)"
    }
}
```
