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
class BinaryNode<Value> {
    let value: Value
    var leftChild: BinaryNode?
    var rightChild: BinaryNode?
    
    init(_ value: Value) {
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

func minBST<CollectionType: RandomAccessCollection>(_ collection: CollectionType) -> BinaryNode<CollectionType.Element>? {
    guard !collection.isEmpty else {
        return nil
    }
    
    let size = collection.distance(from: collection.startIndex, to: collection.endIndex)
    let middleIndex = collection.index(collection.startIndex, offsetBy: size / 2)
    let middleValue = collection[middleIndex]
    
    let root = BinaryNode(middleValue)
    root.leftChild = minBST(collection[..<middleIndex])
    root.rightChild = minBST(collection[collection.index(after: middleIndex)...])
    return root
}

minBST([1, 2, 3, 4, 5, 6, 7, 8, 9])?.inOrder()
```
