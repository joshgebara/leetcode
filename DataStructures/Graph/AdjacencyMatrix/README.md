# Adjacency Matrix

```swift
class AdjacencyMatrix<Element>: Graph {
    var vertices: [Vertex<Element>] = []
    var weights: [[Double?]] = []
    
    func createVertex(data: Element) -> Vertex<Element> {
        let vertex = Vertex(data: data, index: vertices.count)
        vertices.append(vertex)
        
        for i in 0..<weights.count {
            weights[i].append(nil)
        }
        
        let row = [Double?](repeating: nil, count: vertices.count)
        weights.append(row)
        return vertex
    }
    
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        weights[source.index][destination.index] = weight
    }
    
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double? {
        return weights[source.index][destination.index]
    }
    
    func edges(from source: Vertex<Element>) -> [Edge<Element>] {
        var edges = [Edge<Element>]()
        
        for column in 0..<weights.count {
            if let weight = weights[source.index][column] {
                let edge = Edge(source: source, destination: vertices[column], weight: weight)
                edges.append(edge)
            }
        }
        
        return edges
    }
}
```

## Graph Protocol
```swift
enum EdgeType {
    case directed
    case undirected
}

protocol Graph {
    associatedtype Element
    func createVertex(data: Element) -> Vertex<Element>
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?)
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double?
    func edges(from source: Vertex<Element>) -> [Edge<Element>]
}

extension Graph {
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        switch edge {
        case .directed:
            addDirectedEdge(from: source, to: destination, weight: weight)
        case .undirected:
            addUndirectedEdge(between: source, and: destination, weight: weight)
        }
    }
    
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?) {
        addDirectedEdge(from: source, to: destination, weight: weight)
        addDirectedEdge(from: destination, to: source, weight: weight)
    }
}
```

## Auxiliary Data Structures

### Vertex

```swift
struct Vertex<Element> {
    let data: Element
    let index: Int
}

extension Vertex: Hashable where Element: Hashable {}
extension Vertex: Equatable where Element: Equatable {}
```

### Edge

```swift
struct Edge<Element> {
    let source: Vertex<Element>
    let destination: Vertex<Element>
    let weight: Double?
}
```
