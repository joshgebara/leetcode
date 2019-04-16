# Depth First Traversal
* Time: ```O(n)```
* Space: ```O(h)``` where ```h``` is the height of the tree

```swift
extension TreeNode {
    func depthFirstTraversal() {
        print(self)
        children.forEach {
            $0.depthFirstTraversal()
        }
    }
}

let node1 = TreeNode(1) // Root
let node2 = TreeNode(2)
let node3 = TreeNode(3)
let node4 = TreeNode(4)
let node5 = TreeNode(5)
let node6 = TreeNode(6)
let node7 = TreeNode(7)

node1.add(node2)
node1.add(node3)

node2.add(node4)
node2.add(node5)
node2.add(node6)

node3.add(node7)

node1.depthFirstTraversal() // 1 2 4 5 6 3 7
```
