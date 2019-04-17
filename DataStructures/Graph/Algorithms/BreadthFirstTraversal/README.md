# Depth First Traversal
* Time: ```O(V + E)```
* Space: ```O(V)```

```swift
extension Graph where Element: Hashable {
    func depthFirstTraversal(from start: Vertex<Element>) -> [Vertex<Element>] {
        var stack = Stack<Vertex<Element>>()
        var pushed = Set<Vertex<Element>>()
        var visited = [Vertex<Element>]()
        
        stack.push(start)
        pushed.insert(start)
        visited.append(start)
        
        outer: while let vertex = stack.peek() {
            let neighborEdges = edges(from: vertex)
            guard !neighborEdges.isEmpty else {
                stack.pop()
                continue
            }
            
            for edge in neighborEdges {
                if !pushed.contains(edge.destination) {
                    stack.push(edge.destination)
                    pushed.insert(edge.destination)
                    visited.append(edge.destination)
                    continue outer
                }
            }
            stack.pop()
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
