# AVL Tree

```swift
struct AVLTree<Value: Comparable> {
    var root: AVLNode<Value>?
    
    var isEmpty: Bool {
        return root == nil
    }
}

extension AVLTree {
    mutating func balanced(_ node: AVLNode<Value>) -> AVLNode<Value> {
        switch node.balanceFactor {
            case 2:
                guard let leftChild = node.leftChild, leftChild.balanceFactor == -1 else {
                    return rightRotate(node)
                }
                return leftRightRotate(node)
            case -2:
                guard let rightChild = node.rightChild, rightChild.balanceFactor == 1 else {
                    return leftRotate(node)
                }
                return rightLeftRotate(node)
            default:
                return node
        }
    }
}

extension AVLTree {
    mutating func leftRotate(_ node: AVLNode<Value>) -> AVLNode<Value> {
        let pivot = node.rightChild!
        node.rightChild = pivot.leftChild
        pivot.leftChild = node
        
        node.height = max(node.leftHeight, node.rightHeight) + 1
        pivot.height = max(pivot.leftHeight, pivot.rightHeight) + 1
        
        return pivot
    }
    
    mutating func rightRotate(_ node: AVLNode<Value>) -> AVLNode<Value> {
        let pivot = node.leftChild!
        node.leftChild = pivot.rightChild
        pivot.rightChild = node
        
        node.height = max(node.leftHeight, node.rightHeight) + 1
        pivot.height = max(pivot.leftHeight, pivot.rightHeight) + 1
        
        return pivot
    }
    
    mutating func leftRightRotate(_ node: AVLNode<Value>) -> AVLNode<Value> {
        guard let leftChild = node.leftChild else {
            return node
        }
        node.leftChild = leftRotate(leftChild)
        return rightRotate(node)
    }
    
    mutating func rightLeftRotate(_ node: AVLNode<Value>) -> AVLNode<Value> {
        guard let rightChild = node.rightChild else {
            return node
        }
        node.rightChild = rightRotate(rightChild)
        return leftRotate(node)
    }
}
```

## Insert
* Time: ```O(log n)```
* Space: ```O(log n)```

```swift
extension AVLTree {
    mutating func insert(_ value: Value) {
        root = insert(from: root, value: value)
    }
    
    mutating func insert(from node: AVLNode<Value>?, value: Value) -> AVLNode<Value> {
        guard let node = node else {
            return AVLNode(value)
        }
        
        if value < node.value {
            node.leftChild = insert(from: node.leftChild, value: value)
        } else {
            node.rightChild = insert(from: node.rightChild, value: value)
        }
        
        let balancedNode = balanced(node)
        balancedNode.height = max(balancedNode.leftHeight, balancedNode.rightHeight) + 1
        return balancedNode
    }
}
```

## Remove
* Time: ```O(log n)```
* Space: ```O(log n)```

```swift
extension AVLTree {
    mutating func remove(_ value: Value) {
        root = remove(from: root, value: value)
    }
    
    mutating func remove(from node: AVLNode<Value>?, value: Value) -> AVLNode<Value>? {
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
        
        let balancedNode = balanced(node)
        balancedNode.height = max(balancedNode.leftHeight, balancedNode.rightHeight) + 1
        return balancedNode
    }
}
```

# AVL Node
```swift
class AVLNode<Value> {
    var value: Value
    var leftChild: AVLNode?
    var rightChild: AVLNode?
    var height = 0
    
    init(_ value: Value) {
        self.value = value
    }
}

extension AVLNode {
    var balanceFactor: Int {
        return leftHeight - rightHeight
    }
    
    var leftHeight: Int {
        return leftChild?.height ?? -1
    }
    
    var rightHeight: Int {
        return rightChild?.height ?? -1
    }
    
    var min: AVLNode<Value> {
        return leftChild?.min ?? self
    }
}

extension AVLNode: CustomStringConvertible {
    var description: String {
        return "\(value)"
    }
}
```
