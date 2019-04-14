# Contains
* Time: ```O(h)``` where ```h``` is the height of the tree
* Space: ```O(1)```

```swift
extension BinarySearchTree {
    func contains(_ value: Value) -> Bool {
        var current = root
        
        while let node = current {
            guard value != node.value else {
                return true
            }
            
            if value < node.value {
                current = node.leftChild
            } else {
                current = node.rightChild
            }
        }
        return false
    }
}
```
