# Heap

```swift
struct Heap<Element: Comparable> {
    let sort: (Element, Element) -> Bool
    var elements: [Element]
    
    init(sort: @escaping (Element, Element) -> Bool = (<), elements: [Element] = []) {
        self.sort = sort
        self.elements = elements
        
        heapify()
    }
}

extension Heap {
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    var count: Int {
        return elements.count
    }
}

extension Heap {
    mutating func heapify() {
        guard !isEmpty else { return }
        
        for index in stride(from: count / 2 - 1, through: 0, by: -1) {
            siftDown(from: index)
        }
    }
}

extension Heap {
    mutating func siftDown(from index: Int) {
        var parent = index
        while true {
            let left = leftChildIndex(ofParentAt: parent)
            let right = rightChildIndex(ofParentAt: parent)
            var candidate = parent
            
            if left < count && sort(elements[left], elements[candidate]) {
                candidate = left
            }
            
            if right < count && sort(elements[right], elements[candidate]) {
                candidate = right
            }
            
            if candidate == parent {
                return
            }
            
            elements.swapAt(parent, candidate)
            parent = candidate
        }
    }
    
    mutating func siftUp(from index: Int) {
        var child = index
        var parent = parentIndex(ofChildAt: child)
        
        while child > 0 && sort(elements[child], elements[parent]) {
            elements.swapAt(child, parent)
            child = parent
            parent = parentIndex(ofChildAt: child)
        }
    }
}

extension Heap {
    func leftChildIndex(ofParentAt index: Int) -> Int {
        return 2 * index + 1
    }
    
    func rightChildIndex(ofParentAt index: Int) -> Int {
        return 2 * index + 2
    }
    
    func parentIndex(ofChildAt index: Int) -> Int {
        return (index - 1) / 2
    }
}

extension Heap {
    mutating func insert(_ element: Element) {
        elements.append(element)
        siftUp(from: count - 1)
    }
    
    mutating func remove() -> Element? {
        guard !isEmpty else { return nil }
        
        elements.swapAt(0, count - 1)
        
        defer {
            siftDown(from: 0)
        }
        
        return elements.popLast()
    }
    
    mutating func remove(at index: Int) -> Element? {
        guard index < count else {
            return nil
        }
        
        if index == count - 1 {
            return elements.popLast()
        } else {
            elements.swapAt(index, count - 1)
            
            defer {
                siftUp(from: index)
                siftDown(from: index)
            }
            
            return elements.popLast()
        }
    }
}

extension Heap: CustomStringConvertible {
    var description: String {
        return elements.description
    }
}

extension Heap: ExpressibleByArrayLiteral {
    init(arrayLiteral: Element...) {
        sort = (<)
        elements = arrayLiteral
        heapify()
    }
}
```
