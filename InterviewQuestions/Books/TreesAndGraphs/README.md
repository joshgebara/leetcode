# Tree and Graph Questions

## Route Between Nodes

**Source:** 
* Cracking the Coding Interview - Chapter 4

**Complexity**
* Time: ```O(V + E)```
* Space: ```O(V)```
```swift
extension Graph where Element: Hashable {
    func route(between start: Vertex<Element>, and end: Vertex<Element>) -> Bool {
        var queue = Queue<Vertex<Element>>()
        var enqueued = Set<Vertex<Element>>()
        
        queue.enqueue(start)
        enqueued.insert(start)
        
        while let vertex = queue.dequeue() {
            for edge in edges(from: vertex) {
                guard edge.destination != end else {
                    return true
                }
                
                if !enqueued.contains(edge.destination) {
                    queue.enqueue(edge.destination)
                    enqueued.insert(edge.destination)
                }
            }
        }
        return false
    }
}
```

## Minimal Tree

**Source:** 
* Cracking the Coding Interview - Chapter 4

**Complexity**
* Time: ```O(n)```
* Space: ```O(n)```
```swift
class BinaryNode {
    let value: Int
    var leftChild: BinaryNode?
    var rightChild: BinaryNode?
    
    init(_ value: Int) {
        self.value = value
    }
}

extension BinaryNode {
    func inOrder() {
        leftChild?.inOrder()
        print(value)
        rightChild?.inOrder()
    }
}

func createMinimalBST(_ elements: [Int]) -> BinaryNode? {
    return createMinimalBST(elements[...])
}

func createMinimalBST(_ elements: ArraySlice<Int>) -> BinaryNode? {
    guard !elements.isEmpty else {
        return nil
    }
    
    let size = elements.distance(from: elements.startIndex, to: elements.endIndex)
    let middleIndex = elements.index(elements.startIndex, offsetBy: size / 2)
    let middleElement = elements[middleIndex]
    let node = BinaryNode(middleElement)
    
    node.leftChild = createMinimalBST(elements[..<middleIndex])
    node.rightChild = createMinimalBST(elements[elements.index(after: middleIndex)...])
    return node
}

createMinimalBST([1, 2, 3, 4, 5, 6, 7, 8, 9])?.inOrder()
```
