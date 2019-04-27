# Stack and Queue Questions

## Three In One

**Source:** 
* Cracking the Coding Interview - Chapter 3

```swift
```

## Stack Min/Max

**Source:** 
* Cracking the Coding Interview - Chapter 3
* Interview Cake - Chapter 7

### O(n) Space
```swift
struct MinStack<Element: Comparable> {
    var elements: [Element] = []
    var mins: [Element] = []
    
    var min: Element? {
        return mins.last
    }
    
    mutating func push(_ element: Element) {
        if let min = mins.last {
            if element <= min {
                mins.append(element)
            }
        } else {
            mins.append(element)
        }
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        let value = elements.popLast()
        if let min = mins.last {
            if value == min {
                mins.popLast()
            }
        }
        return value
    }
}


var stack = Stack<Int>()
stack.push(3)
stack.push(4)
stack.push(2)
stack.push(8)
stack.push(3)
stack.push(5)
stack.push(1)
stack.mins
stack.min
stack.pop()
stack.mins
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
```

### O(1) Space
```swift
struct MinStack {
    var base = Stack<Int>()
    var minElement: Int?

    mutating func push(_ element: Int) {
        if minElement == nil {
            minElement = element
        }

        guard element < minElement! else {
            base.push(element)
            return
        }

        let value = 2 * element - minElement!
        minElement = element
        base.push(value)
    }

    mutating func pop() -> Int? {
        guard let value = base.pop() else {
            minElement = nil
            return nil
        }

        guard let min = minElement else {
            return nil
        }

        guard value >= min else {
            defer {
                minElement = 2 * min - value
            }
            return minElement
        }

        return value
    }

    var min: Int? {
        return minElement
    }
}

struct Stack<Element> {
    var elements: [Element] = []

    mutating func push(_ element: Element) {
        elements.append(element)
    }

    mutating func pop() -> Element? {
        return elements.popLast()
    }

    var isEmpty: Bool {
        return elements.isEmpty
    }
}

var stack = MinStack()
stack.push(5)
stack.min
stack.push(4)
stack.min
stack.push(6)
stack.min
stack.push(1)
stack.min
stack.push(3)
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
stack.pop()
stack.min
```

## Stack of Plates

**Source:** 
* Cracking the Coding Interview - Chapter 3

```swift
struct SetOfStacks<Element> {
    private var stacks: [Stack<Element>] = []
    private let capacity: Int
    
    init(with capacity: Int) {
        self.capacity = capacity
    }
    
    var isEmpty: Bool {
        return stacks.isEmpty
    }
    
    var count: Int {
        var count =  0
        for stack in stacks {
            count += stack.count
        }
        return count
    }
    
    private var stackCount: Int {
        return stacks.count
    }

    mutating func push(_ element: Element) {
        guard var lastStack = stacks.last, !lastStack.atCapacity else {
            var stack = Stack<Element>(with: capacity)
            stack.push(element)
            return stacks.append(stack)
        }
        lastStack.push(element)
    }
    
    mutating func pop() -> Element? {
        guard var lastStack = stacks.last else {
            return nil
        }

        defer {
            if lastStack.isEmpty {
                _ = stacks.popLast()
            }
        }

        return lastStack.pop()
    }
    
    mutating func pop(at index: Int) -> Element? {
        guard index < stackCount else {
            return nil
        }
        
        var stack = stacks[index]
        
        defer {
            if stack.isEmpty {
                _ = stacks.remove(at: index)
            }
        }

        return stack.pop()
    }

    func peek() -> Element? {
        guard let lastStack = stacks.last else {
            return nil
        }

        return lastStack.peek()
    }
}

extension SetOfStacks: CustomStringConvertible {
    var description: String {
        return stacks.description
    }
}

class Stack<Element> {
    private var elements: [Element] = []
    private let capacity: Int
    
    init(with capacity: Int) {
        elements.reserveCapacity(capacity)
        self.capacity = capacity
    }
    
    var atCapacity: Bool {
        return elements.count >= capacity
    }
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    var count: Int {
        return elements.count
    }
    
    func peek() -> Element? {
        return elements.last
    }
    
    func push(_ element: Element) {
        guard !atCapacity else {
            return
        }
        elements.append(element)
    }
    
    func pop() -> Element? {
        return elements.popLast()
    }
}

extension Stack: CustomStringConvertible {
    var description: String {
        return elements.description
    }
}

var stacks = SetOfStacks<Int>(with: 3)
stacks
stacks.push(1)
stacks.push(2)
stacks.push(3)
stacks.push(4)
stacks.push(5)
stacks.push(6)
stacks.push(7)
stacks.push(8)
stacks.push(9)
stacks.push(10)
stacks.push(11)
stacks.push(12)
stacks.push(13)
stacks.peek()
stacks.pop(at: 2)
stacks.pop(at: 2)
stacks.pop(at: 2)
stacks.pop(at: 2)
stacks.pop(at: 2)
stacks.pop(at: 2)
stacks.pop(at: 2)
stacks.pop()
stacks.pop()
stacks.pop()
stacks.pop()
stacks.pop()
stacks.pop()
stacks.pop()
stacks.pop()
```

## Queue via Stacks

**Source:** 
* Cracking the Coding Interview - Chapter 3
* Interview Cake - Chapter 7

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
            return leftStack.first
        } else {
            return rightStack.last
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

## Sort Stack

**Source:** 
* Cracking the Coding Interview - Chapter 3

```swift
```

## Animal Shelter

**Source:** 
* Cracking the Coding Interview - Chapter 3

```swift
```

## Parenthesis Matching

**Source:** 
* Interview Cake - Chapter 7

```swift
```

## Bracket Validator

**Source:** 
* Interview Cake - Chapter 7

```swift
```
