# Stack

```swift
struct Stack<Element> {
    var elements: [Element] = []
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
    
    func peek() -> Element? {
        return elements.last
    }
}

extension Stack: ExpressibleByArrayLiteral {
    init(arrayLiteral: Element...) {
        for element in arrayLiteral {
            elements.append(element)
        }
    }
}

extension Stack: CustomStringConvertible {
    var description: String {
        return elements.description
    }
}
```
