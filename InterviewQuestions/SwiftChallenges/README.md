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

## LazyScanSequence

```swift
struct LazyScanIterator<Base: IteratorProtocol, Result>: IteratorProtocol {
    var nextElement: Result?
    var base: Base
    let nextPartialResult: (Result, Base.Element) -> Result
    
    mutating func next() -> Result? {
        return nextElement.map { result in
            nextElement = base.next().map { nextPartialResult(result, $0) }
            return result
        }
    }
}

struct LazyScanSequence<Base: Sequence, Result>: LazySequenceProtocol {
    let initial: Result
    let base: Base
    let nextPartialResult: (Result, Base.Element) -> Result
    
    func makeIterator() -> LazyScanIterator<Base.Iterator, Result> {
        return LazyScanIterator(nextElement: initial, base: base.makeIterator(), nextPartialResult: nextPartialResult)
    }
}

extension LazySequenceProtocol {
    func scan<Result>(_ initial: Result, _ nextPartialResult: @escaping (Result, Element) -> Result) -> LazyScanSequence<Self, Result> {
        return LazyScanSequence(initial: initial, base: self, nextPartialResult: nextPartialResult)
    }
}

let numbers = [1, 1, 1, 1, 1, 1, 1, 1, 1, 1]
let accumulatedNumbers = numbers.lazy
                                .scan(0, +)

for number in accumulatedNumbers {
    print(number)
}
```
