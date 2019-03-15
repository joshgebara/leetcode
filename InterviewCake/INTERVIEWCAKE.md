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