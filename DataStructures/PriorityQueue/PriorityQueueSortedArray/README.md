# Priority Queue - With Sorted Array

```swift
struct PriorityQueue<Element: Comparable> {
    var elements: SortedArray<Element>
    
    init(sort: @escaping (Element, Element) -> Bool = (<), elements: [Element] = []) {
        self.elements = SortedArray<Element>(areInIncreasingOrder: sort, elements: elements)
    }
    
    mutating func enqueue(_ element: Element) {
        elements.insert(element)
    }
    
    mutating func dequeue() -> Element? {
        return elements.popLast()
    }
    
    func peek() -> Element? {
        return elements.last
    }
}
```

## Auxiliary Data Structures

### Sorted Array
```swift
struct SortedArray<Element: Comparable> {
    typealias Comparator = (Element, Element) -> Bool
    private let areInIncreasingOrder: Comparator
    private var _elements: [Element]
    
    init(areInIncreasingOrder: @escaping Comparator = (<), elements: [Element] = []) {
        self.areInIncreasingOrder = areInIncreasingOrder
        _elements = elements
        sort()
    }
    
    mutating func append(_ element: Element) {
        defer {
            sort()
        }
        _elements.append(element)
    }
    
    mutating func popLast() -> Element? {
        defer {
            sort()
        }
        return _elements.popLast()
    }
    
    private mutating func sort() {
        _elements.sort(by: areInIncreasingOrder)
    }
}

extension SortedArray: RandomAccessCollection {
    var startIndex: Int {
        return _elements.startIndex
    }
    
    var endIndex: Int {
        return _elements.endIndex
    }
    
    func index(after i: Int) -> Int {
        return _elements.index(after: i)
    }
    
    func index(before i: Int) -> Int {
        return _elements.index(before: i)
    }
    
    subscript(position: Int) -> Element {
        return _elements[position]
    }
}

extension SortedArray: CustomStringConvertible {
    var description: String {
        return _elements.description
    }
}
```
