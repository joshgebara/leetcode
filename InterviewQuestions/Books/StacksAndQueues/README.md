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

### Min Stack - O(1) Space
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
```

## Queue via Stacks

**Source:** 
* Cracking the Coding Interview - Chapter 3
* Interview Cake - Chapter 7

```swift
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
