## Merging Meeting Times

```swift
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