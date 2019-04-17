# Adjacency Set

```swift
class AdjacencySet<Element: Hashable>: Graph {
    var adjacencies: [Vertex<Element>: Set<Edge<Element>>] = [:]
    
    func createVertex(data: Element) -> Vertex<Element> {
        let vertex = Vertex(index: adjacencies.count, data: data)
        adjacencies[vertex] = Set<Edge<Element>>()
        return vertex
    }
    
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        let edge = Edge(source: source, destination: destination, weight: weight)
        adjacencies[source]?.insert(edge)
    }
    
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double? {
        return edges(from: source)
                .first { $0.destination == destination }?
                .weight
    }
    
    func edges(from source: Vertex<Element>) -> [Edge<Element>] {
        return Array(adjacencies[source] ?? [])
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

extension Edge: Equatable where Element: Equatable {}
extension Edge: Hashable where Element: Hashable {}
```
