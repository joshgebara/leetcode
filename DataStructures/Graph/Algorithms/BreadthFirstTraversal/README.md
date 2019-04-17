# Breadth First Traversal
* Time: ```O(V + E)```, where ```V``` is the number of vertices and ```E``` is the number of edges
* Space: ```O(V)```, where ```V``` is the number of vertices

```swift
extension Graph where Element: Hashable {
    func breadthFirstTraversal(from start: Vertex<Element>) -> [Vertex<Element>] {
        var queue = Queue<Vertex<Element>>()
        var enqueued = Set<Vertex<Element>>()
        var visited = [Vertex<Element>]()
        
        queue.enqueue(start)
        enqueued.insert(start)
        
        while let vertex = queue.dequeue() {
            visited.append(vertex)
            
            for edge in edges(from: vertex) {
                if !enqueued.contains(edge.destination) {
                    queue.enqueue(edge.destination)
                    enqueued.insert(edge.destination)
                }
            }
        }
        return visited
    }
}
```

## Auxiliary Data Structures

### Queue

```swift
struct Queue<Element> {
    private var leftStack: Stack<Element> = []
    private var rightStack: Stack<Element> = []
    
    var isEmpty: Bool {
        return leftStack.isEmpty && rightStack.isEmpty
    }

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
```

### Stack
```swift
struct Stack<Element> {
    private var elements: [Element] = []
    
    var isEmpty: Bool {
        return elements.isEmpty
    }

    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}
```
