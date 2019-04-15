# Stack

```swift
struct Stack<Element> {
    var elements: [Element] = []
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}
```
