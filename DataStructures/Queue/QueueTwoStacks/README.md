# Queue - With Two Stacks

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

    func peek() -> Element? {
        if rightStack.isEmpty {
            return leftStack.last
        } else {
            return rightStack.first
        }
    }
}

extension Queue: ExpressibleByArrayLiteral {
    init(arrayLiteral: Element...) {
        for element in arrayLiteral {
            enqueue(element)
        }
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
    
    var first: Element? {
        return elements.first
    }
    
    var last: Element? {
        return elements.last
    }
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}

extension Stack: ExpressibleByArrayLiteral {
    init(arrayLiteral: Element...) {
        for element in arrayLiteral {
            push(element)
        }
    }
}

extension Stack: CustomStringConvertible {
    var description: String {
        return elements.description
    }
}
```
