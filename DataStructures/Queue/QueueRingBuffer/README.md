# Queue - With Ring Buffer

```swift
struct Queue<Element> {
    private var elements: RingBuffer<Element>
    
    init(capacity: Int) {
        elements = RingBuffer(capacity: capacity)
    }

    var isEmpty: Bool {
        return elements.isEmpty
    }

    mutating func enqueue(_ element: Element) {
        elements.write(element)
    }

    mutating func dequeue() -> Element? {
        return elements.read()
    }

    func peek() -> Element? {
        return elements.peek()
    }
}

extension Queue: CustomStringConvertible {
    var description: String {
        return elements.description
    }
}
```

## Auxiliary Data Structures

### Ring Buffer
```swift
struct RingBuffer<Element> {
    private var elements: [Element?]
    private var writeIndex = 0
    private var readIndex = 0
    
    init(capacity: Int) {
        elements = [Element?](repeating: nil, count: capacity)
    }
    
    @discardableResult
    mutating func write(_ element: Element) -> Bool {
        guard !isFull  else {
            return false
        }
        
        elements[writeIndex % elements.count] = element
        writeIndex += 1
        return true
    }
    
    mutating func read() -> Element? {
        guard !isEmpty else {
            return nil
        }
        
        let element = elements[readIndex % elements.count]
        readIndex += 1
        return element
    }
    
    func peek() -> Element? {
        return elements[readIndex % elements.count]
    }
}

extension RingBuffer {
    var count: Int {
        return avaliableSpaceForReading
    }
    
    var isEmpty: Bool {
        return avaliableSpaceForReading == 0
    }
    
    var isFull: Bool {
        return avaliableSpaceForWriting == 0
    }
    
    private var avaliableSpaceForWriting: Int {
        return elements.count - avaliableSpaceForReading
    }
    
    private var avaliableSpaceForReading: Int {
        return writeIndex - readIndex
    }
}

extension RingBuffer: CustomStringConvertible {
    var description: String {
        return elements.description
    }
}
```
