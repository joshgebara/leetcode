# Binary Search Tree

```swift
struct BinarySearchTree<Value: Comparable> {
    var root: BinaryNode<Value>?
}
```

## Insert
* Time: ```O(h)``` where ```h``` is the height of the tree
* Space: ```O(h)``` where ```h``` is the height of the tree

```swift
extension BinarySearchTree {
    mutating func insert(_ value: Value) {
        root = insert(from: root, value: value)
    }
    
    mutating func insert(from node: BinaryNode<Value>?, value: Value) -> BinaryNode<Value>? {
        guard let node = node else {
            return BinaryNode(value: value)
        }
        
        if value < node.value {
            node.leftChild = insert(from: node.leftChild, value: value)
        } else {
            node.rightChild = insert(from: node.rightChild, value: value)
        }
        
        return node
    }
}
```

## Remove
* Time: ```O(h)``` where ```h``` is the height of the tree
* Space: ```O(h)``` where ```h``` is the height of the tree

```swift
extension BinarySearchTree {
    mutating func remove(_ value: Value) {
        root = remove(from: root, value: value)
    }
    
    mutating func remove(from node: BinaryNode<Value>?, value: Value) -> BinaryNode<Value>? {
        guard let node = node else {
            return nil
        }
        
        if value == node.value {
            if node.leftChild == nil && node.rightChild == nil {
                return nil
            }
            
            if node.leftChild == nil {
                return node.rightChild
            }
            
            if node.rightChild == nil {
                return node.leftChild
            }
            
            node.value = node.rightChild!.min.value
            node.rightChild = remove(from: node.rightChild, value: node.value)
            
        } else if value < node.value {
            node.leftChild = remove(from: node.leftChild, value: value)
        } else {
            node.rightChild = remove(from: node.rightChild, value: value)
        }
        
        return node
    }
}
```

# Binary Node
```swift
class BinaryNode<Value> {
    var value: Value
    var leftChild: BinaryNode?
    var rightChild: BinaryNode?

    var min: BinaryNode {
        return leftChild?.min ?? self
    }

    init(value: Value) {
        self.value = value
    }
}

extension BinaryNode: CustomStringConvertible {
    var description: String {
        return "\(value)"
    }
}
```
