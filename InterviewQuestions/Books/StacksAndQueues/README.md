# Stack and Queue Questions

## Three In One

**Source:** 
* Cracking the Coding Interview - Chapter 3

```swift
struct MultiStack<Element> {
    var base: [Element?] = []
    var sizes: [Int] = []
    
    var numberOfStacks: Int
    var capacity: Int
    
    init(with numberOfStacks: Int, capacity: Int) {
        base = [Element?](repeating: nil, count: numberOfStacks * capacity)
        sizes = [Int](repeating: 0, count: numberOfStacks)
        
        self.numberOfStacks = numberOfStacks
        self.capacity = capacity
    }
    
    mutating func push(_ element: Element, onTo stackNumber: Int) {
        guard !isFull(stackNumber) else {
            return
        }
        sizes[stackNumber] += 1
        base[indexOfTop(stackNumber)] = element
    }

    mutating func pop(from stackNumber: Int) -> Element? {
        guard !isEmpty(in: stackNumber) else {
            return nil
        }
        
        defer {
            base[indexOfTop(stackNumber)] = nil
            sizes[stackNumber] -= 1
        }
        return base[indexOfTop(stackNumber)]
    }

    mutating func peek(on stackNumber: Int) -> Element? {
        return base[indexOfTop(stackNumber)]
    }

    mutating func isEmpty(in stackNumber: Int) -> Bool {
        return sizes[stackNumber] == 0
    }

    func isFull(_ stackNumber: Int) -> Bool {
        return sizes[stackNumber] == capacity
    }

    func indexOfTop(_ stackNumber: Int) -> Int {
        let offset = stackNumber * capacity
        let size = sizes[stackNumber]
        return offset + size - 1
    }
}

var stack = MultiStack<Int>(with: 3, capacity: 5)
stack.push(1, onTo: 0)
stack.push(2, onTo: 1)
stack.push(3, onTo: 2)
stack.push(4, onTo: 0)
stack.push(5, onTo: 2)
stack.push(6, onTo: 2)
stack.peek(on: 0)
stack.peek(on: 1)
print(stack)
stack.peek(on: 2)
print(stack)
stack.pop(from: 2)
print(stack)
stack.pop(from: 2)
print(stack)
stack.pop(from: 2)
print(stack)
stack.pop(from: 2)
print(stack)
stack.pop(from: 2)
print(stack)
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
struct Stack<Element> {
    var elements: [Element] = []
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    func peek() -> Element? {
        return elements.last
    }
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}

struct SortedStack<Element: Comparable> {
    var base = Stack<Element>()
    
    mutating func push(_ element: Element) {
        base.push(element)
    }
    
    mutating func pop() -> Element? {
        return base.pop()
    }
    
    mutating func peek() -> Element? {
        return base.peek()
    }

    mutating func sort() {
        var bufferStack = Stack<Element>()
        
        while !base.isEmpty {
            guard let temp = base.pop() else {
                return
            }
            
            while let last = bufferStack.peek(), last > temp {
                base.push(bufferStack.pop()!)
            }
            
            bufferStack.push(temp)
        }
        
        while let value = bufferStack.pop() {
            base.push(value)
        }
        
    }

    var isEmpty: Bool {
        return base.isEmpty
    }
}


var sortedStack = SortedStack<Int>()
sortedStack.push(10)
sortedStack.push(1)
sortedStack.push(5)
sortedStack.push(6)
sortedStack.push(20)
sortedStack.push(3)
print(sortedStack)
sortedStack.sort()
print(sortedStack)
```

## Animal Shelter

**Source:** 
* Cracking the Coding Interview - Chapter 3

```swift
import Foundation

struct AnimalQueue {
    var dogQueue = Queue<AnimalQueueNode<Dog>>()
    var catQueue = Queue<AnimalQueueNode<Cat>>()
    
    mutating func enqueue(_ animal: Animal) {
        if let dog = animal as? Dog {
            let node = AnimalQueueNode(dog)
            dogQueue.enqueue(node)
            return
        }
        
        if let cat = animal as? Cat {
            let node = AnimalQueueNode(cat)
            catQueue.enqueue(node)
            return
        }
    }
    
    mutating func dequeueAny() -> Animal? {
        let nextDog = dogQueue.peek()
        let nextCat = catQueue.peek()
        
        guard nextDog != nil && nextCat != nil else {
            return nil
        }
        
        guard nextDog != nil else {
            return dequeueCat()
        }
        
        guard nextCat != nil else {
            return dequeueDog()
        }
        
        if nextDog!.enqueuedDate > nextCat!.enqueuedDate {
            return dequeueCat()
        } else {
            return dequeueDog()
        }
    }
    
    mutating func dequeueDog() -> Dog? {
        return dogQueue.dequeue()?.animal
    }
    
    mutating func dequeueCat() -> Cat? {
        return catQueue.dequeue()?.animal
    }
}

struct AnimalQueueNode<AnimalType: Animal> {
    var animal: AnimalType
    var enqueuedDate: Date
    
    init(_ animal: AnimalType) {
        self.animal = animal
        enqueuedDate = Date()
    }
}

protocol Animal {
    var name: String { get }
}

struct Dog: Animal {
    var name: String
}

extension Dog: CustomStringConvertible {
    var description: String {
        return "\(name)"
    }
}

struct Cat: Animal {
    var name: String
}

extension Cat: CustomStringConvertible {
    var description: String {
        return "\(name)"
    }
}

struct Queue<Element> {
    var leftStack = Stack<Element>()
    var rightStack = Stack<Element>()
    
    mutating func peek() -> Element? {
        shiftStacks()
        return rightStack.peek()
    }
    
    var isEmpty: Bool {
        return leftStack.isEmpty && rightStack.isEmpty
    }
    
    mutating func enqueue(_ element: Element) {
        leftStack.push(element)
    }
    
    mutating func dequeue() -> Element? {
        shiftStacks()
        return rightStack.pop()
    }
    
    mutating func shiftStacks() {
        if rightStack.isEmpty {
            while let value = leftStack.pop() {
                rightStack.push(value)
            }
        }
    }
}

struct Stack<Element> {
    var elements: [Element] = []
    
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    func peek() -> Element? {
        return elements.last
    }
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}

var animals = AnimalQueue()

animals.enqueue(Dog(name: "A"))
animals.enqueue(Dog(name: "B"))
animals.enqueue(Cat(name: "C"))
animals.enqueue(Dog(name: "D"))
animals.enqueue(Dog(name: "E"))
animals.enqueue(Dog(name: "F"))
animals.enqueue(Cat(name: "G"))
animals.enqueue(Cat(name: "H"))
animals.enqueue(Cat(name: "I"))

animals.dequeueAny()
animals.dequeueDog()
animals.dequeueDog()
animals.dequeueDog()
animals.dequeueAny()
animals.dequeueAny()
animals.dequeueCat()
animals.dequeueCat()
animals.dequeueDog()
animals.dequeueAny()
```

## Parenthesis Matching

**Source:** 
* Interview Cake - Chapter 7

```swift
extension String {
    func getClosingParen(for position: Index) -> Index? {
        guard "(" == self[position] else {
            return nil
        }
        
        var openParenCount = 1
        
        for index in self[position...].indices.dropFirst() {
            let character = self[index]
            
            if "(" == character {
                openParenCount += 1
            } else if ")" == character {
                openParenCount -= 1
                
                if openParenCount == 0 {
                    return index
                }
            }
        }
        
        return nil
    }
}

let sentence = "Sometimes (when I nest them (my parentheticals) too much (like this (and this))) they get confusing."
let position = sentence.getClosingParen(for: sentence.index(sentence.startIndex, offsetBy: 10))
sentence[position!]
```

## Bracket Validator

**Source:** 
* Interview Cake - Chapter 7

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

extension String {
    func validBrackets() -> Bool {
        var stack = Stack<Element>()
        let openersToClosers: [Character: Character] = ["(": ")",
                                                        "[": "]",
                                                        "{": "}"]
        
        let openers = Set<Character>(openersToClosers.keys)
        let closers = Set<Character>(openersToClosers.values)
        
        for character in self {
            if openers.contains(character) {
                stack.push(character)
                continue
            }
            
            if closers.contains(character) {
                guard let top = stack.pop() else {
                    return false
                }
                
                if openersToClosers[top] != character {
                    return false
                }
            }
        }
        return stack.isEmpty
    }
}

"{ [ ] ( ) }".validBrackets()
"{ [ ( ] ) }".validBrackets()
"{ [ }".validBrackets()
```
