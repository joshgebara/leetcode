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
            for edge in graph.edges(from: vertex) {
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
