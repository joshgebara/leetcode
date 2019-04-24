# Linked List Questions

## Remove Dups

**Source:** 
* Cracking the Coding Interview - Chapter 2

### Using temporary buffer

```swift
```

### Without temporary buffer

```swift
```

## Return Kth to Last

**Source:** 
* Cracking the Coding Interview - Chapter 2
* Interview Cake - Chapter 8
* Programming Interviews Exposed - Chapter 4

```swift
```

## Delete Middle Node

**Source:** 
* Cracking the Coding Interview - Chapter 2
* Interview Cake - Chapter 8

```swift
```

## Partition

**Source:** 
* Cracking the Coding Interview - Chapter 2

```swift
```

## Sum Lists

**Source:** 
* Cracking the Coding Interview - Chapter 2

### Reverse Order Linked Lists

```swift
```

### Forward Order Linked Lists

```swift
```

## Palindrome

**Source:** 
* Cracking the Coding Interview - Chapter 2

```swift
```

## Intersection

**Source:** 
* Cracking the Coding Interview - Chapter 2

```swift
```

## Loop Detection

**Source:** 
* Cracking the Coding Interview - Chapter 2
* Interview Cake - Chapter 8
* Programming Interviews Exposed - Chapter 4

### Return Bool - O(1) Space
```swift
class Node<Value> {
    let value: Value
    var next: Node?

    init(_ value: Value, next: Node? = nil) {
        self.value = value
        self.next = next
    }
}

extension Node {
    var isCyclic: Bool {
        var fast: Node? = self
        var slow: Node? = self
        
        while fast != nil && fast?.next != nil {
            fast = fast?.next?.next
            slow = slow?.next
            
            if fast === slow {
                return true
            }
        }
        return false
    }
}

var node1 = Node(1)
var node2 = Node(2)
var node3 = Node(3)
var node4 = Node(4)
var node5 = Node(5)
var node6 = Node(6)
var node7 = Node(7)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7
node7.next = node4

node1.isCyclic // true
```

### Return Bool - O(n) Space

```swift
class Node<Value> {
    let value: Value
    var next: Node?
    
    init(_ value: Value, next: Node? = nil) {
        self.value = value
        self.next = next
    }
}

extension Node {
    var isCyclic: Bool {
        var current: Node? = self
        var seen = Set<Node>()
        
        while let node = current?.next {
            guard !seen.contains(node) else {
                return true
            }
            
            current = node.next
            seen.insert(node)
        }
        return false
    }
}

extension Node: PointerHashable {}

protocol PointerHashable: class, Hashable {}

extension PointerHashable {
    static func == (lhs: Self, rhs: Self) -> Bool {
        return lhs === rhs
    }
    
    func hash(into hasher: inout Hasher) {
        hasher.combine(ObjectIdentifier(self))
    }
}

var node1 = Node(1)
var node2 = Node(2)
var node3 = Node(3)
var node4 = Node(4)
var node5 = Node(5)
var node6 = Node(6)
var node7 = Node(7)

node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5
node5.next = node6
node6.next = node7
node7.next = node4

node1.isCyclic // true
```

### Return Node at Beginning of the Loop

```swift

```

### Would the program always work if the fast runner moves three steps every time the slow runner moves one step?

## Reverse a Linked List

**Source:** 
* Interview Cake - Chapter 8

### Iterative

```swift
```

### Recursive

```swift

```

## Stack Implementation

**Source:** 
* Programming Interviews Exposed - Chapter 4

```swift

```

## Queue Implementation

**Source:** 
* Data Structures & Algorithms in Swift

```swift

```

## Multilevel List

**Source:** 
* Programming Interviews Exposed - Chapter 4

### Flatten the List
```swift

```


### Unflatten the List
```swift

```

## Implement a Singly Linked List
* ```push()```
* ```append()```
* ```insert(after:)```
* ```pop()```
* ```removeLast()```
* ```remove(after:)```
* ```node(at:)```

**Source:** 
* Data Structures & Algorithms in Swift

```swift

```
