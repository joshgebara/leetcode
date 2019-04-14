# In-Order Traversal

* Time: ```O(n)```
* Space: ```O(h)``` where ```h``` is the height of the tree

```swift
extension BinaryNode {
    func postOrderTraversal() {
        leftChild?.postOrderTraversal()
        rightChild?.postOrderTraversal()
        print(self)
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

seven.postOrderTraversal() // 0 5 1 8 9 7
```
