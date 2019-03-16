## Merging Meeting Times

```swift

// Time: O(n log n)
// Space - O(n)

class Meeting: CustomStringConvertible {
    var startTime: Int
    var endTime: Int

    init(startTime: Int, endTime: Int) {
        self.startTime = startTime
        self.endTime = endTime
    }

    var description: String {
        return "(\(startTime), \(endTime))"
    }
}

extension Array where Element == Meeting {
    func mergeRanges() -> [Meeting] {
        guard !isEmpty else { return [] }
        
        let sortedMeetings = sorted { $0.startTime < $1.startTime }
        var mergedMeetings = [sortedMeetings[0]]

        for meeting in sortedMeetings.dropFirst() {
            guard let lastMergedMeeting = mergedMeetings.last else { break }

            if meeting.startTime <= lastMergedMeeting.endTime {
                lastMergedMeeting.endTime = Swift.max(lastMergedMeeting.endTime, meeting.endTime)
            } else {
                mergedMeetings.append(meeting)
            }
        }
        return mergedMeetings
    }
}


let meetings =   [
    Meeting(startTime: 0,  endTime: 1),
    Meeting(startTime: 3,  endTime: 5),
    Meeting(startTime: 4,  endTime: 8),
    Meeting(startTime: 10, endTime: 12),
    Meeting(startTime: 9,  endTime: 10)
]

print(meetings.mergeRanges())

```

## Reverse String in Place

```swift

// Time: O(n)
// Space - O(1)

extension String {
    mutating func reverse() {
        var leftIndex = startIndex
        var rightIndex = index(before: endIndex)
        
        while leftIndex < rightIndex {
            let first = self[leftIndex]
            let second = self[rightIndex]

            replaceSubrange(leftIndex...leftIndex, with: String(second))
            replaceSubrange(rightIndex...rightIndex, with: String(first))

            leftIndex = index(after: leftIndex)
            rightIndex = index(before: rightIndex)
        }
    }
}

var a = "Taco"
a.reverse()

```

## Reverse Words

```swift

// Time: O(n)
// Space - O(1)

func reverseCharacters(_ string: inout String, from startIndex: String.Index, until endIndex: String.Index) {
    guard startIndex != endIndex else { return }

    var leftIndex  = startIndex
    var rightIndex = string.index(before: endIndex)

    while leftIndex < rightIndex {
        let leftChar  = string[leftIndex]
        let rightChar = string[rightIndex]

        string.replaceSubrange(leftIndex...leftIndex, with: String(rightChar))
        string.replaceSubrange(rightIndex...rightIndex, with: String(leftChar))

        leftIndex  = string.index(after: leftIndex)
        rightIndex = string.index(before: rightIndex)
    }
}

func reverseWords(_ message: inout String) {
    reverseCharacters(&message, from: message.startIndex, until: message.endIndex)
    
    var currentWordStartIndex = message.startIndex
    
    for i in message.indices {
        if message[i] == " " {
            reverseCharacters(&message, from: currentWordStartIndex, until: i)
            currentWordStartIndex = message.index(after: i)
        }
    }
    
    reverseCharacters(&message, from: currentWordStartIndex, until: message.endIndex)
}


var a = "the eagle has landed"
reverseWords(&a)
print(a)

```

## Merge Sorted Arrays

```swift

// Time: O(n)
// Space - O(n)

let myArray = [3, 4, 6, 10, 11, 15]
let alicesArray = [1, 5, 8, 12, 14, 19]

print(mergeArrays(myArray, alicesArray))

func mergeArrays<Element: Comparable>(_ left: [Element], _ right: [Element]) -> [Element] {
    var result = [Element]()
    var leftIndex = 0
    var rightIndex = 0
    
    while left.count > leftIndex && right.count > rightIndex {
        let leftElement = left[leftIndex]
        let rightElement = right[rightIndex]
        
        if leftElement > rightElement {
            result.append(rightElement)
            rightIndex += 1
        } else if leftElement < rightElement {
            result.append(leftElement)
            leftIndex += 1
        } else {
            result.append(leftElement)
            result.append(rightElement)
            leftIndex += 1
            rightIndex += 1
        }
    }
    
    if left.count > leftIndex {
        result.append(contentsOf: left[leftIndex...])
    } else {
        result.append(contentsOf: right[rightIndex...])
    }
    
    return result
}

```

## Merge K Sorted Arrays

```swift

// Time: O(kn log k)
// Space: O(kn)

struct Heap<Element: Comparable> {
    let sort: (Element, Element) -> Bool
    var elements: [Element] = []
    
    init(sort: @escaping (Element, Element) -> Bool, elements: [Element] = []) {
        self.sort = sort
        self.elements = elements
        
        if !isEmpty {
            for i in stride(from: count / 2 - 1, through: 0, by: -1) {
                siftDown(from: i)
            }
        }
    }
    
    var count: Int {
        return elements.count
    }
    
    var isEmpty: Bool {
        return elements.isEmpty
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
            elements.swapAt(parent, child)
            child = parent
            parent = parentIndex(ofChildAt: child)
        }
    }
}

struct PriorityQueue<Element: Comparable> {
    var elements: Heap<Element>
    
    init(sort: @escaping (Element, Element) -> Bool = (<), elements: [Element] = []) {
        self.elements = Heap<Element>(sort: sort, elements: elements)
    }
    
    mutating func enqueue(_ element: Element) {
        elements.insert(element)
    }
    
    mutating func dequeue() -> Element? {
        return elements.remove()
    }
}

struct QueueElement<Element: Comparable> {
    var arrayIndex: Int
    var elementIndex: Int
    var value: Element
}

extension QueueElement: Comparable & Equatable {
    static func < (lhs: QueueElement, rhs: QueueElement) -> Bool {
        return lhs.value < rhs.value
    }
    
    static func == (lhs: QueueElement, rhs: QueueElement) -> Bool {
        return lhs.value == rhs.value
    }
}

func merge<Element: Comparable>(_ arrays: [[Element]]) -> [Element] {
    var priorityQueue = PriorityQueue<QueueElement<Element>>()
    var result = [Element]()
    
    for index in arrays.indices {
        guard !arrays[index].isEmpty else { continue }
        
        let queueElement = QueueElement(arrayIndex: index,
                                        elementIndex: 0,
                                        value: arrays[index][0])
        priorityQueue.enqueue(queueElement)
    }
    
    while let element = priorityQueue.dequeue() {
        result.append(element.value)
        
        var nextIndex = element.elementIndex + 1
        if nextIndex < arrays[element.arrayIndex].count {
            let queueElement = QueueElement(arrayIndex: element.arrayIndex,
                                            elementIndex: nextIndex,
                                            value: arrays[element.arrayIndex][nextIndex])
            priorityQueue.enqueue(queueElement)
        }
        
    }
    return result
}

var a = [
    [1, 2, 3],
    [9, 10, 11, 13, 15],
    [7, 8, 9]
]

print(merge(a))

```

## Single Riffle

```swift
// Time: O(n)
// Space: O(1)

func isSingleRiffle(_ shuffledDeck: [Int], _ half1: [Int], _ half2: [Int]) -> Bool {
    var half1Index = 0
    var half2Index = 0

    for card in shuffledDeck {
        if half1Index < half1.count, card == half1[half1Index] {
            half1Index += 1
            continue
        }
        
        if half2Index < half2.count, card == half2[half2Index] {
            half2Index += 1
            continue
        }
        
        return false
    }
    return true
}

isSingleRiffle(Array(1...51), Array(1...26), Array(27...51))

```

## Inflight Entertainment

```swift
// Time: O(n)
// Space: O(n)

extension Array where Element: Numeric & Hashable  {
    func twoSum(for target: Element) -> Bool {
        var elementSet = Set<Element>()
        
        for element in self {
            let compliment = target - element
            if elementSet.contains(compliment) {
                return true
            }
            elementSet.insert(element)
        }
        return false
    }
}

[90, 91, 120, 180, 123, 154, 122, 164].twoSum(for: 180)
```

## Inflight Entertainment - Guaranteed Sort

```swift
// Time: O(n)
// Space: O(1)

extension Array where Element == Int  {
    func twoSum(for target: Element) -> Bool {
        var left = startIndex
        var right = index(before: endIndex)
        
        while left <= right {
            let sum = self[left] + self[right]
            
            if sum == target {
                return true
            }
            
            if sum > target {
                right = index(before: right)
            } else {
                left = index(after: left)
            }
        }
        return false
    }
}

[90, 91, 120, 123, 154, 164, 180].twoSum(for: 210)

```