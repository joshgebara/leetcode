# Pre-Order Traversal

```swift
extension BinaryNode {
    func preOrderTraversal() {
        print(self)
        leftChild?.preOrderTraversal()
        rightChild?.preOrderTraversal()
    }
}

let zero  = BinaryNode(0)
let one   = BinaryNode(1)
let five  = BinaryNode(5)
let seven = BinaryNode(7) // Root
let eight = BinaryNode(8)
let nine  = BinaryNode(9)

seven.leftChild  = one
one.leftChild    = zero
one.rightChild   = five
seven.rightChild = nine
nine.leftChild   = eight

seven.preOrderTraversal() // 7 1 0 5 9 8
```
