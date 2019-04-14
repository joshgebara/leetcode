# Level Order Traversal

```swift
struct Stack<Element> {
    var elements = [Element]()
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    mutating func push(_ element: Element) {
        return elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}

struct Queue<Element> {
    var leftStack = Stack<Element>()
    var rightStack = Stack<Element>()
    
    mutating func enqueue(_ element: Element) {
        leftStack.push(element)
    }
    
    mutating func dequeue() -> Element? {
        if rightStack.isEmpty {
            while let element = leftStack.pop() {
                rightStack.push(element)
            }
        }
        return rightStack.pop()
    }
}

extension TreeNode {
    func levelOrderTraversal() {
        var queue = Queue<TreeNode>()
        queue.enqueue(self)
        
        while let node = queue.dequeue() {
            print(node)
            for child in node.children {
                queue.enqueue(child)
            }
        }
    }
}

let node1 = TreeNode(1)
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

node1.levelOrderTraversal() // 1 2 3 4 5 6 7
```
