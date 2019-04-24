
# Depth First Traversal
* Time: ```O(V + E)```, where ```V``` is the number of vertices and ```E``` is the number of edges
* Space: ```O(V)```, where ```V``` is the number of vertices

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
