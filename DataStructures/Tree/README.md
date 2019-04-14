# Tree

```swift
class TreeNode<Value> {
    let value: Value
    var children: [TreeNode] = []
    
    init(_ value: Value) {
        self.value = value
    }
}

extension TreeNode: CustomStringConvertible {
    var description: String {
        return "\(value)"
    }
}

extension TreeNode {
    func add(_ child: TreeNode) {
        children.append(child)
    }
}
```
