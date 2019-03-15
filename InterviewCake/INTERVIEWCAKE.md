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