## LazyEveryOtherSequence
```swift
struct LazyEveryOtherSequence<Base: Sequence>: LazySequenceProtocol {
    let base: Base
    
    func makeIterator() -> LazyEveryOtherIterator<Base.Iterator> {
        return LazyEveryOtherIterator(base: base.makeIterator())
    }
}

struct LazyEveryOtherIterator<Base: IteratorProtocol>: IteratorProtocol {
    var base: Base
    
    mutating func next() -> Base.Element? {
        _ = base.next()
        return base.next()
    }
}

extension LazySequenceProtocol {
    func everyOther() -> LazyEveryOtherSequence<Self> {
        return LazyEveryOtherSequence(base: self)
    }
}

let numbers = (1...)
                .lazy
                .map { $0 * $0 }
                .prefix { $0 < 100 }
                .everyOther()

for number in numbers {
    print(number)
}
```
