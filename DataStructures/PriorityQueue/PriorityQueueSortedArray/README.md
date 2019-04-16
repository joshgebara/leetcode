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
    let areInIncreasingOrder: (Element, Element) -> Bool
    var elements: [Element]
    
    init(areInIncreasingOrder: @escaping (Element, Element) -> Bool = (<), elements: [Element] = []) {
        self.areInIncreasingOrder = areInIncreasingOrder
        
        let sortedElements = elements.sorted { !areInIncreasingOrder($0, $1) }
        self.elements = sortedElements
    }
    
    var last: Element? {
        return elements.last
    }
    
    mutating func insert(_ element: Element) {
        elements.append(element)
        
        let sortedElements = elements.sorted { !areInIncreasingOrder($0, $1) }
        elements = sortedElements
    }
    
    mutating func popLast() -> Element? {
        defer {
            let sortedElements = elements.sorted { !areInIncreasingOrder($0, $1) }
            elements = sortedElements
        }
        
        return elements.popLast()
    }
}
```
