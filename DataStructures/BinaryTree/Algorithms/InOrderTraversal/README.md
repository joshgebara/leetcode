# In-Order Traversal

* Time: ```O(n)```
* Space: ```O(h)``` where ```h``` is the height of the tree

```swift
extension BinaryNode {
    func inOrderTraversal() {
        leftChild?.inOrderTraversal()
        print(self)
        rightChild?.inOrderTraversal()
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

seven.inOrderTraversal() // 0 1 5 7 8 9
```
