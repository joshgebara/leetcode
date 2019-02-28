# swift-algorithms

### Base

```swift
// Linked List
// Stack - Array
// Stack - LinkedList
// Queue - Array
// Queue - Linked List
// Queue - Ring Buffer
// Queue - Two Stacks
// Ring Buffer
// Tree
// Binary Tree
// Binary Search Tree
// AVL Tree
// Trie
// Binary Search - Iterative
// Binary Search - Recursive
// Heap
// Priority Queue
// Bubble Sort
// Selection Sort
// Insertion Sort
// Merge Sort
// Radix Sort
// Counting Sort
// Bucket Sort
// Heap Sort
// Quick Sort
// Graph - AdjacencyList
// Graph - AdjacencyMatrix
// Graph - AdjacencySet
// Graph - Breadth-First Search
// Graph - Depth-First Search
// Graph - Dijkstra's Algorithms
// Graph - Prim's Algorithm
```

## Linked List

```swift
class Node<Value> {
    let value: Value
    var next: Node<Value>?
    
    init(_ value: Value, next: Node<Value>? = nil) {
        self.value = value
        self.next = next
    }
}

extension Node: CustomStringConvertible {
    var description: String {
        guard let next = next else {
            return "\(value)"
        }
        return "\(value) -> \(next)"
    }
}

struct LinkedList<Value> {
    var head: Node<Value>?
    var tail: Node<Value>?
}

extension LinkedList: CustomStringConvertible {
    var description: String {
        guard let head = head else {
            return ""
        }
        return "\(head)"
    }
}

extension LinkedList {
    var isEmpty: Bool {
        return head == nil
    }
}

extension LinkedList {
    mutating func push(_ value: Value) {
        head = Node(value, next: head)
        if tail == nil {
            tail = head
        }
    }
    
    mutating func append(_ value: Value) {
        guard !isEmpty else {
            push(value)
            return
        }
        tail?.next = Node(value)
        tail = tail?.next
    }
    
    mutating func insert(_ value: Value, after node: Node<Value>) {
        guard node !== tail else {
            append(value)
            return
        }
        node.next = Node(value, next: node.next)
    }
}

extension LinkedList {
    func node(at index: Int) -> Node<Value>? {
        var currentIndex = 0
        var currentNode = head
        
        while currentNode != nil && currentIndex < index {
            currentNode = currentNode?.next
            currentIndex += 1
        }
        return currentNode
    }
}

extension LinkedList {
    @discardableResult
    mutating func pop() -> Node<Value>? {
        defer {
            head = head?.next
            if isEmpty {
                tail = nil
            }
        }
        return head
    }
    
    @discardableResult
    mutating func removeLast() -> Node<Value>? {
        guard let head = head else {
            return nil
        }
        
        guard head.next != nil else {
            return pop()
        }
        
        var current = head
        var previous = head
        
        while let next = current.next {
            previous = current
            current = next
        }
        
        previous.next = nil
        tail = previous
        return current
    }
    
    @discardableResult
    mutating func remove(after node: Node<Value>) -> Node<Value>? {
        defer {
            if node.next === tail {
                tail = node
            }
            node.next = node.next?.next
        }
        return node.next
    }
}
```

## Stack - Array

```swift
struct Stack<Element> {
    var elements: [Element] = []
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}
```

## Stack - LinkedList

```swift 
class Node<Value> {
    let value: Value
    var next: Node<Value>?
    
    init(_ value: Value, next: Node<Value>? = nil) {
        self.value = value
        self.next = next
    }
}

extension Node: CustomStringConvertible {
    var description: String {
        guard let next = next else {
            return "\(value)"
        }
        return "\(value) -> \(next)"
    }
}

struct LinkedList<Value> {
    var head: Node<Value>?
    var tail: Node<Value>?
}

extension LinkedList {
    mutating func push(_ value: Value) {
        head = Node(value, next: head)
        if tail == nil {
            tail = head
        }
    }
    
    @discardableResult
    mutating func pop() -> Value? {
        defer {
            head = head?.next
            if head == nil {
                tail = nil
            }
        }
        return head?.value
    }
}

extension LinkedList: CustomStringConvertible {
    var description: String {
        guard let head = head else {
            return ""
        }
        return "\(head)"
    }
}

struct Stack<Element> {
    var elements = LinkedList<Element>()
    
    mutating func push(_ element: Element) {
        elements.push(element)
    }
    
    mutating func pop() -> Element? {
        return elements.pop()
    }
}
```

## Queue - Array

```swift
struct Queue<Element> {
    var elements: [Element] = []
    
    mutating func enqueue(_ element: Element) {
        elements.append(element)
    }
    
    mutating func dequeue() -> Element? {
        return elements.isEmpty ? nil : elements.removeFirst()
    }
}
```

## Queue - Linked List

```swift
class Node<Value> {
    let value: Value
    var next: Node<Value>?
    
    init(_ value: Value, next: Node<Value>? = nil) {
        self.value = value
        self.next = next
    }
}

extension Node: CustomStringConvertible {
    var description: String {
        guard let next = next else {
            return "\(value)"
        }
        return "\(value) -> \(next)"
    }
}

struct LinkedList<Value> {
    var head: Node<Value>?
    var tail: Node<Value>?
    
    var isEmpty: Bool {
        return head == nil
    }
}

extension LinkedList {
    mutating func pop() -> Value? {
        defer {
            head = head?.next
            if isEmpty {
                tail = nil
            }
        }
        return head?.value ?? nil
    }
    
    mutating func append(_ value: Value) {
        guard !isEmpty else {
            head = Node(value, next: head?.next)
            if tail == nil {
                tail = head
            }
            return
        }
        tail?.next = Node(value)
        tail = tail?.next
    }
}

struct Queue<Element> {
    var elements = LinkedList<Element>()
    
    mutating func enqueue(_ element: Element) {
        elements.append(element)
    }
    
    mutating func dequeue() -> Element? {
        return elements.pop()
    }
}
```

## Queue - Ring Buffer

```swift
struct RingBuffer<T> {
    var array: [T?] = []
    var writeIndex = 0
    var readIndex = 0
    
    init(count: Int) {
        array = [T?](repeating: nil, count: count)
    }
}

extension RingBuffer {
    mutating func write(_ element: T) -> Bool {
        if !isFull {
            array[writeIndex % array.count] = element
            writeIndex += 1
            return true
        } else {
            return false
        }
    }
    
    mutating func read() -> T? {
        if !isEmpty {
            let element = array[readIndex % array.count]
            readIndex += 1
            return element
        } else {
            return nil
        }
    }
}

extension RingBuffer {
    var isEmpty: Bool {
        return avaliableSpaceForReading == 0
    }
    
    var isFull: Bool {
        return avaliableSpaceForWriting == 0
    }
    
    var avaliableSpaceForWriting: Int {
        return array.count - avaliableSpaceForReading
    }
    
    var avaliableSpaceForReading: Int {
        return writeIndex - readIndex
    }
}

struct Queue<Element> {
    var ringBuffer: RingBuffer<Element>
    
    init(count: Int) {
        ringBuffer = RingBuffer<Element>(count: count)
    }
    
    mutating func enqueue(_ element: Element) -> Bool {
        return ringBuffer.write(element)
    }
    
    mutating func dequeue() -> Element? {
        return ringBuffer.read()
    }
}
```

## Queue - Two Stacks

```swift 
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

struct Queue<Element> {
    var leftStack = Stack<Element>()
    var rightStack = Stack<Element>()
    
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
}
```

## Ring Buffer

```swift
struct RingBuffer<T> {
    var array: [T?] = []
    var writeIndex = 0
    var readIndex = 0
    
    init(count: Int) {
        array = [T?](repeating: nil, count: count)
    }
}

extension RingBuffer {
    mutating func write(_ element: T) -> Bool {
        if !isFull {
            array[writeIndex % array.count] = element
            writeIndex += 1
            return true
        } else {
            return false
        }
    }
    
    mutating func read() -> T? {
        if !isEmpty {
            let element = array[readIndex % array.count]
            readIndex += 1
            return element
        } else {
            return nil
        }
    }
}

extension RingBuffer {
    var isEmpty: Bool {
        return avaliableSpaceForReading == 0
    }
    
    var isFull: Bool {
        return avaliableSpaceForWriting == 0
    }
    
    var avaliableSpaceForWriting: Int {
        return array.count - avaliableSpaceForReading
    }
    
    var avaliableSpaceForReading: Int {
        return writeIndex - readIndex
    }
}
```

## Tree

```swift
class TreeNode<Value> {
    let value: Value
    var children: [TreeNode<Value>] = []
    
    init(_ value: Value) {
        self.value = value
    }
}

extension TreeNode: CustomStringConvertible {
    var description: String {
        return "\(value)"
    }
}

extension TreeNode {
    func add(_ child: TreeNode<Value>) {
        children.append(child)
    }
}

extension TreeNode {
    func depthFirst() {
        print(self)
        for child in children {
            child.depthFirst()
        }
    }
    
    func levelOrder() {
        print(self)
        var queue = Queue<TreeNode<Value>>()
        
        for child in children {
            queue.enqueue(child)
        }
        
        while let node = queue.dequeue() {
            print(node)
            for child in node.children {
                queue.enqueue(child)
            }
        }
    }
}

struct Queue<Element> {
    var elements: [Element] = []
    
    mutating func enqueue(_ element: Element) {
        elements.append(element)
    }
    
    mutating func dequeue() -> Element? {
        return elements.isEmpty ? nil : elements.removeFirst()
    }
}
```

## BinaryTree

```swift
class BinaryNode<Value> {
    let value: Value
    var leftChild: BinaryNode?
    var rightChild: BinaryNode?
    
    init(value: Value) {
        self.value = value
    }
}

extension BinaryNode: CustomStringConvertible {
    var description: String {
        return "\(value)"
    }
}

extension BinaryNode {
    func preOrder() {
        print(self)
        leftChild?.preOrder()
        rightChild?.preOrder()
    }
    
    func inOrder() {
        leftChild?.inOrder()
        print(self)
        rightChild?.inOrder()
    }
    
    func postOrder() {
        leftChild?.postOrder()
        rightChild?.postOrder()
        print(self)
    }
}
```

## Binary Search Tree
```swift
class BinaryNode<Value> {
    var value: Value
    var leftChild: BinaryNode?
    var rightChild: BinaryNode?
    
    init(value: Value) {
        self.value = value
    }
}

extension BinaryNode {
    var min: BinaryNode {
        return leftChild?.min ?? self
    }
}

extension BinaryNode: CustomStringConvertible {
    var description: String {
        return "\(value)"
    }
}

struct BinarySearchTree<Value: Comparable> {
    var root: BinaryNode<Value>?
}

extension BinarySearchTree {
    mutating func insert(_ value: Value) {
        root = insert(from: root, value: value)
    }
    
    mutating func insert(from node: BinaryNode<Value>?, value: Value) -> BinaryNode<Value>? {
        guard let node = node else {
            return BinaryNode(value: value)
        }
        
        if value < node.value {
            node.leftChild = insert(from: node.leftChild, value: value)
        } else {
            node.rightChild = insert(from: node.rightChild, value: value)
        }
        return node
    }
}

extension BinarySearchTree {
    mutating func remove(_ value: Value) {
        root = remove(from: root, value: value)
    }
    
    mutating func remove(from node: BinaryNode<Value>?, value: Value) -> BinaryNode<Value>? {
        guard let node = node else {
            return nil
        }
        
        if value == node.value {
            if node.leftChild == nil && node.rightChild == nil {
                return nil
            }
            
            if node.leftChild == nil {
                return node.rightChild
            }
            
            if node.rightChild == nil {
                return node.leftChild
            }
            
            node.value = node.rightChild!.min.value
            node.rightChild = remove(from: node.rightChild, value: node.value)
        } else if value < node.value {
            node.leftChild = remove(from: node.leftChild, value: value)
        } else {
            node.rightChild = remove(from: node.rightChild, value: value)
        }
        return node
    }
}

extension BinarySearchTree {
    func contains(_ value: Value) -> Bool {
        var current = root
        
        while let node = current {
            if value == node.value {
                return true
            }
            
            if value < node.value {
                current = node.leftChild
            } else {
                current = node.rightChild
            }
        }
        return false
    }
}
```

## AVL Tree

```swift
class AVLNode<Value> {
    var value: Value
    var leftChild: AVLNode<Value>?
    var rightChild: AVLNode<Value>?
    var height = 0
    
    init(_ value: Value) {
        self.value = value
    }
}

extension AVLNode {
    var balanceFactor: Int {
        return leftHeight - rightHeight
    }
    
    var leftHeight: Int {
        return leftChild?.height ?? -1
    }
    
    var rightHeight: Int {
        return rightChild?.height ?? -1
    }
    
    var min: AVLNode<Value> {
        return leftChild?.min ?? self
    }
}

struct AVLTree<Value: Comparable> {
    var root: AVLNode<Value>?
}

extension AVLTree {
    mutating func remove(_ value: Value) {
        root = remove(from: root, value: value)
    }
    
    mutating func remove(from node: AVLNode<Value>?, value: Value) -> AVLNode<Value>? {
        guard let node = node else {
            return nil
        }
        
        if value == node.value {
            if node.leftChild == nil && node.rightChild == nil {
                return nil
            }
            
            if node.leftChild == nil {
                return node.rightChild
            }
            
            if node.rightChild == nil {
                return node.leftChild
            }
            
            node.value = node.rightChild!.min.value
            node.rightChild = remove(from: node.rightChild, value: node.value)
        } else if value < node.value {
            node.leftChild = remove(from: node.leftChild, value: value)
        } else {
            node.rightChild = remove(from: node.rightChild, value: value)
        }
        
        let balancedNode = balanced(node)
        balancedNode.height = max(balancedNode.leftHeight, balancedNode.rightHeight) + 1
        return balancedNode
    }
}

extension AVLTree {
    mutating func insert(_ value: Value) {
        root = insert(from: root, value: value)
    }
    
    mutating func insert(from node: AVLNode<Value>?, value: Value) -> AVLNode<Value>? {
        guard let node = node else {
            return AVLNode(value)
        }
        
        if value < node.value {
            node.leftChild = insert(from: node.leftChild, value: value)
        } else {
            node.rightChild = insert(from: node.rightChild, value: value)
        }
        let balancedNode = balanced(node)
        balancedNode.height = max(balancedNode.leftHeight, balancedNode.rightHeight) + 1
        return balancedNode
    }
}

extension AVLTree {
    mutating func balanced(_ node: AVLNode<Value>) -> AVLNode<Value> {
        switch node.balanceFactor {
            case 2:
                if let leftChild = node.leftChild, leftChild.balanceFactor == -1 {
                    return leftRightRotate(node)
                } else {
                    return rightRotate(node)
                }
            case -2:
                if let rightChild = node.rightChild, rightChild.balanceFactor == 1 {
                    return rightLeftRotate(node)
                } else {
                    return leftRotate(node)
                }
            default:
                return node
        }
    }
}

extension AVLTree {
    mutating func leftRotate(_ node: AVLNode<Value>) -> AVLNode<Value> {
        let pivot = node.rightChild!
        node.rightChild = pivot.leftChild
        pivot.leftChild = node
        
        node.height = max(node.leftHeight, node.rightHeight) + 1
        pivot.height = max(pivot.leftHeight, pivot.rightHeight) + 1
        
        return pivot
    }
    
    mutating func rightRotate(_ node: AVLNode<Value>) -> AVLNode<Value> {
        let pivot = node.leftChild!
        node.leftChild = pivot.rightChild
        pivot.rightChild = node
        
        node.height = max(node.leftHeight, node.rightHeight) + 1
        pivot.height = max(pivot.leftHeight, pivot.rightHeight) + 1
        
        return pivot
    }
    
    mutating func leftRightRotate(_ node: AVLNode<Value>) -> AVLNode<Value> {
        guard let leftChild = node.leftChild else {
            return node
        }
        node.leftChild = leftRotate(leftChild)
        return rightRotate(node)
    }
    
    mutating func rightLeftRotate(_ node: AVLNode<Value>) -> AVLNode<Value> {
        guard let rightChild = node.rightChild else {
            return node
        }
        node.rightChild = rightRotate(rightChild)
        return leftRotate(node)
    }
}

```

## Trie

```swift
class TrieNode<Key: Hashable> {
    var key: Key?
    weak var parent: TrieNode?
    var children: [Key: TrieNode] = [:]
    var isTerminating = false
    
    init(key: Key?, parent: TrieNode?) {
        self.key = key
        self.parent = parent
    }
}

struct Trie<CollectionType: Collection> where CollectionType.Element: Hashable {
    typealias Node = TrieNode<CollectionType.Element>
    var root = Node(key: nil, parent: nil)
}

extension Trie {
    mutating func insert(_ collection: CollectionType) {
        var current = root
        
        for element in collection {
            if current.children[element] == nil {
                current.children[element] = Node(key: element, parent: current)
            }
            current = current.children[element]!
        }
        current.isTerminating = true
    }
    
    mutating func remove(_ collection: CollectionType) {
        var current = root
        
        for element in collection {
            guard let child = current.children[element] else {
                return
            }
            current = child
        }
        
        guard current.isTerminating else {
            return
        }
        
        current.isTerminating = false
        
        while let parent = current.parent, current.children.isEmpty && !current.isTerminating {
            parent.children[current.key!] = nil
            current = parent
        }
    }
    
    mutating func contains(_ collection: CollectionType) -> Bool {
        var current = root
        
        for element in collection {
            guard let child = current.children[element] else {
                return false
            }
            current = child
        }
        return current.isTerminating
    }
}

extension Trie where CollectionType: RangeReplaceableCollection {
    func collections(startingWith prefix: CollectionType) -> [CollectionType] {
        var current = root
        
        for element in prefix {
            guard let child = current.children[element] else {
                return []
            }
            current = child
        }
        return collections(startingWith: prefix, after: current)
    }
    
    func collections(startingWith prefix: CollectionType, after node: Node) -> [CollectionType] {
        var results = [CollectionType]()
        
        if node.isTerminating {
            results.append(prefix)
        }
        
        for child in node.children.values {
            var prefix = prefix
            prefix.append(child.key!)
            results.append(contentsOf: collections(startingWith: prefix, after: child))
        }
        
        return results
    }
}
```

## Binary Search Iterative

```swift
extension RandomAccessCollection where Element: Comparable {
    func binarySearchIterative(for key: Element) -> Bool {
        guard count > 0 else { return false }
        
        var leftIndex = startIndex
        var rightIndex = index(before: endIndex)
        
        while leftIndex <= rightIndex {
            let size = distance(from: leftIndex, to: rightIndex)
            let middleIndex = index(leftIndex, offsetBy: size / 2)
            let middleValue = self[middleIndex]
            
            if middleValue == key {
                return true
            }
            
            if middleValue < key {
                leftIndex = index(after: middleIndex)
            }
            
            if middleValue > key {
                rightIndex = index(before: middleIndex)
            }
        }
        return false
    }
}
```

## Binary Search Recursive
```swift
extension RandomAccessCollection where Element: Comparable {
    func binarySearchRecursive(for key: Element, in range: Range<Index>? = nil) -> Bool {
        let range = range ?? startIndex..<endIndex
        guard range.lowerBound < range.upperBound else { return false }
        let size = distance(from: range.lowerBound, to: range.upperBound)
        let middleIndex = index(range.lowerBound, offsetBy: size / 2)
        let middleValue = self[middleIndex]
        
        if middleValue == key {
            return true
        }
        
        if middleValue < key {
            return binarySearchRecursive(for: key, in: index(after: middleIndex)..<range.upperBound)
        }
        
        if middleValue > key {
            return binarySearchRecursive(for: key, in: range.lowerBound..<middleIndex)
        }
        
        return false
    }
}
```

## Heap 

```swift
struct Heap<Element> {
    let sort: (Element, Element) -> Bool
    var elements: [Element]
    
    init(sort: @escaping (Element, Element) -> Bool, elements: [Element] = []) {
        self.sort = sort
        self.elements = elements
        
        if !elements.isEmpty {
            for i in stride(from: count / 2 - 1, through: 0, by: -1) {
                siftDown(from: i)
            }
        }
    }
}

extension Heap {
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    var count: Int {
        return elements.count
    }
    
    func peek() -> Element? {
        return elements.first
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
    mutating func remove() -> Element? {
        guard !isEmpty else { return nil }
        
        elements.swapAt(0, count - 1)
        
        defer {
            siftDown(from: 0)
        }
        
        return elements.popLast()
    }
    
    mutating func insert(_ element: Element) {
        elements.append(element)
        siftUp(from: count - 1)
    }
    
    mutating func remove(at index: Int) -> Element? {
        guard index < count else { return nil }
        
        if index == count - 1 {
            return elements.removeLast()
        } else {
            elements.swapAt(index, count - 1)
            
            defer {
                siftDown(from: index)
                siftUp(from: index)
            }
            
            return elements.removeLast()
        }
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
            
            elements.swapAt(candidate, parent)
            parent = candidate
        }
    }
    
    mutating func siftUp(from index: Int) {
        var child = index
        var parent = parentIndex(ofChildAt: child)
        
        while child > 0 && sort(elements[child], elements[parent]) {
            elements.swapAt(child, parent)
            child = parent
            parent = parentIndex(ofChildAt: child)
        }
    }
}
```

## PriorityQueue

```swift
struct Heap<Element: Comparable> {
    let sort: (Element, Element) -> Bool
    var elements: [Element]
    
    init(sort: @escaping (Element, Element) -> Bool, elements: [Element]) {
        self.sort = sort
        self.elements = elements
        
        if !elements.isEmpty {
            for index in stride(from: count / 2 - 1, through: 0, by: -1) {
                siftDown(from: index)
            }
        }
    }
}

extension Heap {
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
    mutating func remove() -> Element? {
        guard !isEmpty else {
            return nil
        }
        
        elements.swapAt(0, count - 1)
        
        defer {
            siftDown(from: 0)
        }
        
        return elements.removeLast()
    }
    
    mutating func insert(_ element: Element) {
        elements.append(element)
        siftUp(from: count - 1)
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
            
            elements.swapAt(candidate, parent)
            parent = candidate
        }
    }
    
    mutating func siftUp(from index: Int) {
        var child = index
        var parent = parentIndex(ofChildAt: child)
        
        while child > 0 && sort(elements[parent], elements[child]) {
            elements.swapAt(parent, child)
            child = parent
            parent = parentIndex(ofChildAt: child)
        }
    }
}

struct PriorityQueue<Element: Comparable> {
    var heap: Heap<Element>
    init(sort: @escaping (Element, Element) -> Bool,
        elements: [Element] = []) {
        heap = Heap(sort: sort, elements: elements)
        }
    }
}

extension PriorityQueue {
    var isEmpty: Bool {
        return heap.isEmpty
    }
}

extension PriorityQueue {
    mutating func enqueue(_ element: Element) {
        heap.insert(element)
    }
    
    mutating func dequeue() -> Element? {
        return heap.remove()
    }
}
```

## Bubble Sort

```swift
// Bubble Sort
// Best - O(n)
// Worst - O(n^2)
// Space - O(1)

extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
    mutating func bubbleSort() {
        bubbleSort(by: <)
    }
    
    mutating func bubbleSort(by areInIncreasingOrder: (Element, Element) throws -> Bool) rethrows {
        guard count > 1 else {
            return
        }
        
        for endingIndex in indices.reversed() {
            var noSwaps = true
            for currentIndex in startIndex..<endingIndex {
                let nextIndex = index(after: currentIndex)
                if try areInIncreasingOrder(self[nextIndex], self[currentIndex]) {
                    swapAt(nextIndex, currentIndex)
                    noSwaps = false
                }
            }
            if noSwaps {
                return
            }
        }
    }
}
```

## Insertion Sort

```swift
// Insertion Sort
// Best - O(n)
// Worst - O(n^2)
// Space - O(1)

extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
    mutating func insertionSort() {
        insertionSort(by: <)
    }
    
    mutating func insertionSort(by areInIncreasingOrder: (Element, Element) throws -> Bool) rethrows {
        guard count > 1 else {
            return
        }
        
        for currentIndex in index(after: startIndex)..<endIndex {
            for shiftingIndex in (index(after: startIndex)...currentIndex).reversed() {
                let previousIndex = index(before: shiftingIndex)
                if try areInIncreasingOrder(self[shiftingIndex], self[previousIndex]) {
                    swapAt(shiftingIndex, previousIndex)
                } else {
                    break
                }
            }
        }
    }
}
```

## Selection Sort
```swift
// Selection Sort
// Best - O(n^2)
// Worst - O(n^2)
// Space - O(1)

extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
    mutating func selectionSort() {
        selectionSort(by: <)
    }
    
    mutating func selectionSort(by areInIncreasingOrder: (Element, Element) throws -> Bool) rethrows {
        guard count > 1 else {
            return
        }
        
        for currentIndex in startIndex..<index(before: endIndex) {
            var lowestValueIndex = currentIndex
            for swapIndex in index(after: currentIndex)..<endIndex {
                if try areInIncreasingOrder(self[swapIndex], self[lowestValueIndex]) {
                    lowestValueIndex = swapIndex
                }
            }
            swapAt(lowestValueIndex, currentIndex)
        }
    }
}

var a = [7, 6, 5, 6, 5, 4, 5, 4, 3, 4, 3, 2, 3, 2, 1]
a.selectionSort()

```

## Merge Sort

```swift
// Merge Sort
// Best - O(n log n)
// Worst - O(n log n)
// Space - O(n)

func mergeSort<Element: Comparable>(_ array: [Element]) -> [Element] {
    guard array.count > 1 else {
        return array
    }
    
    let middleIndex = array.count / 2
    let left = mergeSort(Array(array[..<middleIndex]))
    let right = mergeSort(Array(array[middleIndex...]))
    return merge(left, right)
}

func merge<Element: Comparable>(_ left: [Element], _ right: [Element]) -> [Element] {
    var results = [Element]()
    var leftIndex = 0
    var rightIndex = 0
    
    while left.count > leftIndex && right.count > rightIndex {
        let leftElement = left[leftIndex]
        let rightElement = right[rightIndex]
        
        if leftElement > rightElement {
            results.append(rightElement)
            rightIndex += 1
        } else if leftElement < rightElement {
            results.append(leftElement)
            leftIndex += 1
        } else {
            results.append(leftElement)
            results.append(rightElement)
            leftIndex += 1
            rightIndex += 1
        }
    }
    
    if left.count > leftIndex {
        results.append(contentsOf: left[leftIndex...])
    } else {
        results.append(contentsOf: right[rightIndex...])
    }
    
    return results
}
```

## Counting Sort

```swift
// Counting Sort
// Best - O(n + k)
// Worst - O(n + k)
// Space - O(n + k)

// Counting Sort
// Best - O(n + k)
// Worst - O(n + k)
// Space - O(n + k)
// Stable

extension Array where Element == Int {
    func countingSort() -> [Element] {
        guard let maxElement = self.max() else { return self }
        
        var buckets: [Element] = .init(repeating: 0, count: maxElement + 1)
        for number in self {
            buckets[number] += 1
        }
        
        for index in buckets.indices.dropFirst() {
            let sum = buckets[index] + buckets[index - 1]
            buckets[index] = sum
        }
        
        var sortedArray: [Element] = .init(repeating: 0, count: count)
        for number in self {
            buckets[number] -= 1
            sortedArray[buckets[number]] = number
        }
        return sortedArray
    }
}

var a = [1, 5, 4, 6, 5, 7, 6, 3, 2, 5, 8, 9, 8, 7, 8, 7, 5, 6, 5, 4, 2]
a.countingSort()



extension Array where Element == Int {
    func countingSort() -> [Element] {
        guard count > 1 else { return self }
        
        let maxElement = self.max() ?? 0
        var countArray = [Element](repeating: 0, count: maxElement + 1)
        for element in self {
            countArray[element] += 1
        }
        
        for index in 1..<countArray.count {
            let sum = countArray[index] + countArray[index - 1]
            countArray[index] = sum
        }
        
        var sortedArray = [Element](repeating: 0, count: count)
        for element in self {
            countArray[element] -= 1
            sortedArray[countArray[element]] = element
        }
        return sortedArray
    }
}
```

## Heap Sort
```swift
// Heap Sort
// Best - O(n log n)
// Worst - O(n log n)
// Space - O(1)

extension Array where Element: Comparable {
    func leftChildIndex(ofParentAt index: Int) -> Int {
        return 2 * index + 1
    }
    
    func rightChildIndex(ofParentAt index: Int) -> Int {
        return 2 * index + 2
    }
    
    mutating func siftDown(from index: Int, upTo size: Int) {
        var parent = index
        while true {
            let left = leftChildIndex(ofParentAt: parent)
            let right = rightChildIndex(ofParentAt: parent)
            var candidate = parent
            
            if left < size && (self[left] > self[candidate]) {
                candidate = left
            }
            
            if right < size && (self[right] > self[candidate]) {
                candidate = right
            }
            
            if candidate == parent {
                return
            }
            
            swapAt(parent, candidate)
            parent = candidate
        }
    }
    
    mutating func heapSort() {
        guard count > 1 else { return }
        
        for index in stride(from: count / 2 - 1, through: 0, by: -1) {
            siftDown(from: index, upTo: count)
        }
        
        for i in indices.reversed() {
            swapAt(0, i)
            siftDown(from: 0, upTo: i)
        }
    }   
}
```

```swift
// Quick Sort
// Best - O(n log n)
// Worst - O(n^2)
// Space - O(1)

func quickSort<Element: Comparable>(_ array: inout [Element]) {
    quickSort(&array, 0, array.count - 1)
}

func quickSort<Element: Comparable>(_ array: inout [Element], _ low: Int, _ high: Int) {
    guard low < high else { return }
    
    let randomIndex = Int.random(in: low...high)
    array.swapAt(randomIndex, high)
    
    let partitionIndex = partition(&array, low, high)
    quickSort(&array, low, partitionIndex - 1)
    quickSort(&array, partitionIndex + 1, high)
}

func partition<Element: Comparable>(_ array: inout [Element], _ low: Int, _ high: Int) -> Int {
  let partitionElement = array[high]
  var lowIndex = low
  
  for currentIndex in low..<high {
    if array[currentIndex] <= partitionElement {
      array.swapAt(currentIndex, lowIndex)
      lowIndex += 1
    }
  }
  
  array.swapAt(lowIndex, high)
  return lowIndex
}
```

## Graph - AdjacencyList

```swift

enum EdgeType {
    case directed
    case undirected
}

protocol Graph {
    associatedtype Element
    func createVertex(data: Element) -> Vertex<Element>
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?)
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func edges(from source: Vertex<Element>) -> [Edge<Element>]
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double?
}

extension Graph {
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?) {
        addDirectedEdge(from: source, to: destination, weight: weight)
        addDirectedEdge(from: destination, to: source, weight: weight)
    }
    
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        switch edge {
            case .directed:
                addDirectedEdge(from: source, to: destination, weight: weight)
            case .undirected:
                addUndirectedEdge(between: source, and: destination, weight: weight)
        }
    }
}

struct Vertex<Element> {
    let index: Int
    let data: Element
}

extension Vertex: Hashable where Element: Hashable {}
extension Vertex: Equatable where Element: Equatable {}

extension Vertex: CustomStringConvertible {
    var description: String {
        return "\(index) - \(data)"
    }
}

struct Edge<Element> {
    let source: Vertex<Element>
    let destination: Vertex<Element>
    let weight: Double?
}

class AdjacencyList<Element: Hashable>: Graph {
    
    var adjacencies: [Vertex<Element>: [Edge<Element>]] = [:]

    func createVertex(data: Element) -> Vertex<Element> {
        let vertex = Vertex(index: adjacencies.count, data: data)
        adjacencies[vertex] = []
        return vertex
    }
    
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        let edge = Edge(source: source, destination: destination, weight: weight)
        adjacencies[source]?.append(edge)
    }
    
    func edges(from source: Vertex<Element>) -> [Edge<Element>] {
        return adjacencies[source] ?? []
    }
    
     func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double? {
        return edges(from: source)
                .first { $0.destination == destination }?
                .weight
    }
}
```

## Priority Queue with AVL Tree

```swift
class AVLNode<Value> {
    var value: Value
    var leftChild: AVLNode<Value>?
    var rightChild: AVLNode<Value>?
    var height = 0
 
    init(_ value: Value) {
        self.value = value
    }
}

extension AVLNode {
    var min: AVLNode<Value> {
        return leftChild?.min ?? self
    }
    
    var max: AVLNode<Value> {
        return rightChild?.max ?? self
    }
}

extension AVLNode {
    var balanceFactor: Int {
        return leftHeight - rightHeight
    }
    
    var leftHeight: Int {
        return leftChild?.height ?? -1
    }
    
    var rightHeight: Int {
        return rightChild?.height ?? -1
    }
}

extension AVLNode: CustomStringConvertible {
    var description: String {
        return "\(value)"
    }
}

struct AVLTree<Value: Comparable> {
    var root: AVLNode<Value>?
    
    var priorityNode: AVLNode<Value>? {
        return root?.max ?? nil
    }
}

extension AVLTree {
    mutating func insert(_ value: Value) {
        root = insert(from: root, value: value)
    }
    
    mutating func insert(from node: AVLNode<Value>?, value: Value) -> AVLNode<Value>? {
        guard let node = node else {
            return AVLNode(value)
        }
        
        if value < node.value {
            node.leftChild = insert(from: node.leftChild, value: value)
        } else {
            node.rightChild = insert(from: node.rightChild, value: value)
        }
        
        let balancedNode = balanced(node)
        balancedNode.height = max(balancedNode.leftHeight, balancedNode.rightHeight) + 1
        return balancedNode
    }
}

extension AVLTree {
    mutating func remove(_ value: Value) {
        root = remove(from: root, value: value)
    }
    
    mutating func remove(from node: AVLNode<Value>?, value: Value) -> AVLNode<Value>? {
        guard let node = node else {
            return nil
        }
        
        if value == node.value {
            if node.leftChild == nil && node.rightChild == nil {
                return nil
            }
            
            if node.leftChild == nil {
                return node.rightChild
            }
            
            if node.rightChild == nil {
                return node.leftChild
            }
            
            node.value = node.rightChild!.min.value
            node.rightChild = remove(from: node.rightChild, value: node.value)
        } else if value < node.value {
            node.leftChild = remove(from: node.leftChild, value: value)
        } else {
            node.rightChild = remove(from: node.rightChild, value: value)
        }
        
        let balancedNode = balanced(node)
        balancedNode.height = max(balancedNode.leftHeight, balancedNode.rightHeight) + 1
        return balancedNode
    }
}

extension AVLTree {
    func contains(_ value: Value) -> Bool {
        var current = root
        
        while let node = current {
            if value == node.value {
                return true
            }
            
            if value < node.value {
                current = node.leftChild
            } else {
                current = node.rightChild
            }
        }
        return false
    }
}

extension AVLTree {
    mutating func balanced(_ node: AVLNode<Value>) -> AVLNode<Value> {
        switch node.balanceFactor {
        case 2:
            if let leftChild = node.leftChild, leftChild.balanceFactor == -1 {
                return leftRightRotate(node)
            } else {
                return rightRotate(node)
            }
        case -2:
            if let rightChild = node.rightChild, rightChild.balanceFactor == 1 {
                return rightLeftRotate(node)
            } else {
                return leftRotate(node)
            }
        default:
            return node
        }
    }
}

extension AVLTree {
    mutating func leftRotate(_ node: AVLNode<Value>) -> AVLNode<Value> {
        let pivot = node.rightChild!
        node.rightChild = pivot.leftChild
        pivot.leftChild = node
    
        node.height = max(node.leftHeight, node.rightHeight) + 1
        pivot.height = max(pivot.leftHeight, pivot.rightHeight) + 1
    
        return pivot
    }
    
    mutating func rightRotate(_ node: AVLNode<Value>) -> AVLNode<Value> {
        let pivot = node.leftChild!
        node.leftChild = pivot.rightChild
        pivot.rightChild = node
    
        node.height = max(node.leftHeight, node.rightHeight) + 1
        pivot.height = max(pivot.leftHeight, pivot.rightHeight) + 1
    
        return pivot
    }
    
    mutating func leftRightRotate(_ node: AVLNode<Value>) -> AVLNode<Value> {
        guard let leftChild = node.leftChild else {
            return node
        }
        node.leftChild = leftRotate(leftChild)
        return rightRotate(node)
    }
    
    mutating func rightLeftRotate(_ node: AVLNode<Value>) -> AVLNode<Value> {
        guard let rightChild = node.rightChild else {
            return node
        }
        node.rightChild = rightRotate(rightChild)
        return leftRotate(node)
    }
}

struct PriorityQueue<Element: Comparable> {
    var elements = AVLTree<Element>()
    
    mutating func enqueue(_ element: Element) {
        elements.insert(element)
    }
    
    mutating func dequeue() -> Element? {
        guard let element = elements.priorityNode else {
            return nil
        }
        
        defer {
            elements.remove(element.value)
        }
        
        return element.value
    }
}
```

## Graph - AdjacencyList

```swift
enum EdgeType {
    case directed
    case undirected
}

protocol Graph {
    associatedtype Element
    func createVertex(data: Element) -> Vertex<Element>
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?)
    func edges(from source: Vertex<Element>) -> [Edge<Element>]
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double?
}

extension Graph {
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        switch edge {
        case .directed:
            addDirectedEdge(from: source, to: destination, weight: weight)
        case .undirected:
            addUndirectedEdge(between: source, and: destination, weight: weight)
        }
    }
    
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?) {
        addDirectedEdge(from: source, to: destination, weight: weight)
        addDirectedEdge(from: destination, to: source, weight: weight)
    }
}

struct Vertex<Element> {
    let index: Int
    let data: Element
}

extension Vertex: Hashable where Element: Hashable {}
extension Vertex: Equatable where Element: Equatable {}

extension Vertex: CustomStringConvertible {
    var description: String {
        return "\(index) - \(data)"
    }
}

struct Edge<Element> {
    let source: Vertex<Element>
    let destination: Vertex<Element>
    let weight: Double?
}

class AdjacencyList<Element: Hashable>: Graph {
    var adjacencies: [Vertex<Element>: [Edge<Element>]] = [:]
    
    func createVertex(data: Element) -> Vertex<Element> {
        let vertex = Vertex(index: adjacencies.count, data: data)
        adjacencies[vertex] = []
        return vertex
    }
    
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        let edge = Edge(source: source, destination: destination, weight: weight)
        adjacencies[source]?.append(edge)
    }
    
    func edges(from source: Vertex<Element>) -> [Edge<Element>] {
        return adjacencies[source] ?? []
    }
    
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double? {
        return edges(from: source)
                .first { $0.destination == destination }?
                .weight
    }
}
```

## Graph - AdjacencyMatrix

```swift
enum EdgeType {
    case directed
    case undirected
}

protocol Graph {
    associatedtype Element
    func createVertex(data: Element) -> Vertex<Element>
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?)
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double?
    func edges(from source: Vertex<Element>) -> [Edge<Element>]
}

extension Graph {
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        switch edge {
        case .directed:
            addDirectedEdge(from: source, to: destination, weight: weight)
        case .undirected:
            addUndirectedEdge(between: source, and: destination, weight: weight)
        }
    }
    
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?) {
        addDirectedEdge(from: source, to: destination, weight: weight)
        addDirectedEdge(from: destination, to: source, weight: weight)
    }
}

struct Vertex<Element> {
    let data: Element
    let index: Int
}

extension Vertex: Equatable where Element: Equatable {}
extension Vertex: Hashable where Element: Hashable {}

extension Vertex: CustomStringConvertible {
    var description: String {
        return "\(index) - \(data)"
    }
}

struct Edge<Element> {
    let source: Vertex<Element>
    let destination: Vertex<Element>
    let weight: Double?
}

class AdjacencyMatrix<Element>: Graph {
    var vertices: [Vertex<Element>] = []
    var weights: [[Double?]] = []
    
    func createVertex(data: Element) -> Vertex<Element> {
        let vertex = Vertex(data: data, index: vertices.count)
        vertices.append(vertex)
        
        for i in 0..<weights.count {
            weights[i].append(nil)
        }
        
        let row = [Double?](repeating: nil, count: vertices.count)
        weights.append(row)
        return vertex
    }
    
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        weights[source.index][destination.index] = weight
    }
    
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double? {
        return weights[source.index][destination.index]
    }
    
    func edges(from source: Vertex<Element>) -> [Edge<Element>] {
        var edges = [Edge<Element>]()
        
        for column in 0..<weights.count {
            if let weight = weights[source.index][column] {
                let edge = Edge(source: source, destination: vertices[column], weight: weight)
                edges.append(edge)
            }
        }
        
        return edges
    }
}
```

## Graph - AdjacencySet

```swift
enum EdgeType {
    case directed
    case undirected
}

protocol Graph {
    associatedtype Element
    func createVertex(data: Element) -> Vertex<Element>
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?)
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double?
    func edges(from source: Vertex<Element>) -> [Edge<Element>]
}

extension Graph {

    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        switch edge {
        case .directed:
            addDirectedEdge(from: source, to: destination, weight: weight)
        case .undirected:
            addUndirectedEdge(between: source, and: destination, weight: weight)
        }
    }
    
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?) {
        addDirectedEdge(from: source, to: destination, weight: weight)
        addDirectedEdge(from: destination, to: source, weight: weight)
    }
}

struct Vertex<Element> {
    let index: Int
    let data: Element
}

extension Vertex: Equatable where Element: Equatable {}
extension Vertex: Hashable where Element: Hashable {}

extension Vertex: CustomStringConvertible {
    var description: String {
        return "\(index) - \(data)"
    }
}

struct Edge<Element> {
    let source: Vertex<Element>
    let destination: Vertex<Element>
    let weight: Double?
}

extension Edge: Equatable where Element: Equatable {}
extension Edge: Hashable where Element: Hashable {}

class AdjacencySet<Element: Hashable>: Graph {
    var adjacencies: [Vertex<Element>: Set<Edge<Element>>] = [:]
    
    func createVertex(data: Element) -> Vertex<Element> {
        let vertex = Vertex(index: adjacencies.count, data: data)
        adjacencies[vertex] = Set<Edge<Element>>()
        return vertex
    }
    
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        let edge = Edge(source: source, destination: destination, weight: weight)
        adjacencies[source]?.insert(edge)
    }
    
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double? {
        return edges(from: source)
                .first { $0.destination == destination }?
                .weight
    }
    
    func edges(from source: Vertex<Element>) -> [Edge<Element>] {
        return Array(adjacencies[source] ?? [])
    }
}
```

## Graph - Breadth First Traversal

```swift
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
    
    func peek() -> Element? {
        return elements.last
    }
}

struct Queue<Element> {
    var leftStack = Stack<Element>()
    var rightStack = Stack<Element>()
    
    mutating func enqueue(_ element: Element) {
        leftStack.push(element)
    }
    
    mutating func dequeue() -> Element? {
        if rightStack.isEmpty {
            while let value = leftStack.pop() {
                rightStack.push(value)
            }
        }
        return rightStack.pop()
    }
}

enum EdgeType {
    case directed
    case undirected
}

protocol Graph {
    associatedtype Element
    func createVertex(data: Element) -> Vertex<Element>
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?)
    func edges(from source: Vertex<Element>) -> [Edge<Element>]
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double?
}

extension Graph {
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        switch edge {
            case .directed:
                addDirectedEdge(from: source, to: destination, weight: weight)
            case .undirected:
                addUndirectedEdge(between: source, and: destination, weight: weight)
        }
    }
    
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?) {
        addDirectedEdge(from: source, to: destination, weight: weight)
        addDirectedEdge(from: destination, to: source, weight: weight)
    }
}

extension Graph where Element: Hashable {
    func breadthFirst(from source: Vertex<Element>) -> [Vertex<Element>] {
        var queue = Queue<Vertex<Element>>()
        var enqueued = Set<Vertex<Element>>()
        var visited = [Vertex<Element>]()
        
        queue.enqueue(source)
        enqueued.insert(source)
        
        while let vertex = queue.dequeue() {
            visited.append(vertex)
            let neighborEdges = edges(from: vertex)
            
            for edge in neighborEdges {
                if !enqueued.contains(edge.destination) {
                    queue.enqueue(edge.destination)
                    enqueued.insert(edge.destination)
                }
            }
        }
        return visited
    }
}

struct Vertex<Element> {
    let index: Int
    let data: Element
}

extension Vertex: Equatable where Element: Equatable {}
extension Vertex: Hashable where Element: Hashable {}

extension Vertex: CustomStringConvertible {
    var description: String {
        return "\(index) - \(data)"
    }
}

struct Edge<Element> {
    let source: Vertex<Element>
    let destination: Vertex<Element>
    let weight: Double?
}

class AdjacencyList<Element: Hashable>: Graph {
    var adjacencies: [Vertex<Element>: [Edge<Element>]] = [:]
    
    func createVertex(data: Element) -> Vertex<Element> {
        let vertex = Vertex(index: adjacencies.count, data: data)
        adjacencies[vertex] = []
        return vertex
    }
    
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        let edge = Edge(source: source, destination: destination, weight: weight)
        adjacencies[source]?.append(edge)
    }
    
    func edges(from source: Vertex<Element>) -> [Edge<Element>] {
        return adjacencies[source] ?? []
    }
    
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double? {
        return edges(from: source).first { $0.destination == destination }?.weight
    }
}
```

## Graph - Depth First

```swift
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
    
    func peek() -> Element? {
        return elements.last
    }
}

struct Queue<Element> {
    var leftStack = Stack<Element>()
    var rightStack = Stack<Element>()
    
    mutating func enqueue(_ element: Element) {
        leftStack.push(element)
    }
    
    mutating func dequeue() -> Element? {
        if rightStack.isEmpty {
            while let value = leftStack.pop() {
                rightStack.push(value)
            }
        }
        return rightStack.pop()
    }
}

enum EdgeType {
    case directed
    case undirected
}

protocol Graph {
    associatedtype Element
    func createVertex(data: Element) -> Vertex<Element>
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?)
    func edges(from source: Vertex<Element>) -> [Edge<Element>]
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double?
}

extension Graph {
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        switch edge {
            case .directed:
                addDirectedEdge(from: source, to: destination, weight: weight)
            case .undirected:
                addUndirectedEdge(between: source, and: destination, weight: weight)
        }
    }
    
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?) {
        addDirectedEdge(from: source, to: destination, weight: weight)
        addDirectedEdge(from: destination, to: source, weight: weight)
    }
}

extension Graph where Element: Hashable {    
    func depthFirst(from source: Vertex<Element>) -> [Vertex<Element>] {
        var stack = Stack<Vertex<Element>>()
        var pushed = Set<Vertex<Element>>()
        var visited = [Vertex<Element>]()
        
        stack.push(source)
        pushed.insert(source)
        visited.append(source)
        
        outer: while let vertex = stack.peek() {
            let neighborEdges = edges(from: vertex)
            guard !neighborEdges.isEmpty else {
                stack.pop()
                continue
            }
            
            for edge in neighborEdges {
                if !pushed.contains(edge.destination) {
                    stack.push(edge.destination)
                    pushed.insert(edge.destination)
                    visited.append(edge.destination)
                    continue outer
                }
            }
            
            stack.pop()
        }
        return visited
    }
}

struct Vertex<Element> {
    let index: Int
    let data: Element
}

extension Vertex: Equatable where Element: Equatable {}
extension Vertex: Hashable where Element: Hashable {}

extension Vertex: CustomStringConvertible {
    var description: String {
        return "\(index) - \(data)"
    }
}

struct Edge<Element> {
    let source: Vertex<Element>
    let destination: Vertex<Element>
    let weight: Double?
}

class AdjacencyList<Element: Hashable>: Graph {
    var adjacencies: [Vertex<Element>: [Edge<Element>]] = [:]
    
    func createVertex(data: Element) -> Vertex<Element> {
        let vertex = Vertex(index: adjacencies.count, data: data)
        adjacencies[vertex] = []
        return vertex
    }
    
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        let edge = Edge(source: source, destination: destination, weight: weight)
        adjacencies[source]?.append(edge)
    }
    
    func edges(from source: Vertex<Element>) -> [Edge<Element>] {
        return adjacencies[source] ?? []
    }
    
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double? {
        return edges(from: source).first { $0.destination == destination }?.weight
    }
}
```

## Dijkstra's Algorithm

```swift
enum Visit<T: Hashable> {
    case start
    case edge(Edge<T>)
}

class Dijkstra<T: Hashable> {
    typealias Graph = AdjacencyList<T>
    let graph: Graph
    
    init(_ graph: Graph) {
        self.graph = graph
    }
}

extension Dijkstra {
    func route(to destination: Vertex<T>, with paths: [Vertex<T>: Visit<T>]) -> [Edge<T>] {
        var vertex = destination
        var path: [Edge<T>] = []
        
        while let visit = paths[vertex], case .edge(let edge) = visit {
            path = [edge] + path
            vertex = edge.source
        }
        
        return path
    }
    
    func distance(to destination: Vertex<T>, with paths: [Vertex<T>: Visit<T>]) -> Double {
        let path = route(to: destination, with: paths)
        let distances = path.compactMap { $0.weight }
        return distances.reduce(0.0, +)
    }
    
    func shortestPath(from start: Vertex<T>) -> [Vertex<T>: Visit<T>] {
        var paths: [Vertex<T>: Visit<T>] = [start: .start]
        var priorityQueue = PriorityQueue<Vertex<T>>(sort: {
            distance(to: $0, with: paths) < distance(to: $1, with: paths)
        })
        priorityQueue.enqueue(start)
        
        while let vertex = priorityQueue.dequeue() {
            for edge in graph.edges(from: vertex) {
                guard let weight = edge.weight else {
                    continue
                }
                
                if paths[edge.destination] == nil ||
                distance(to: vertex, with: paths) + weight < distance(to: edge.destination, with: paths) {
                    paths[edge.destination] = .edge(edge)
                    priorityQueue.enqueue(edge.destination)
                }
            }
        }
        return paths
    }
    
    func shortestPath(to destination: Vertex<T>, with paths: [Vertex<T>: Visit<T>]) -> [Edge<T>] {
        return route(to: destination, with: paths)
    }
}

```


## Prim's Algorithm

```swift
struct Heap<Element> {
    let sort: (Element, Element) -> Bool
    var elements: [Element]
    
    init(sort: @escaping (Element, Element) -> Bool, elements: [Element] = []) {
        self.sort = sort
        self.elements = elements
        
        if !isEmpty {
            for index in stride(from: count / 2 - 1, through: 0, by: -1) {
                siftDown(from: index)
            }
        }
    }
}

extension Heap {
    var isEmpty: Bool {
        return elements.isEmpty
    }
    
    var count: Int {
        return elements.count
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
            elements.swapAt(child, parent)
            child = parent
            parent = parentIndex(ofChildAt: child)
        }
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
        guard !isEmpty else {
            return nil
        }
        
        elements.swapAt(0, count - 1)
        
        defer {
            siftDown(from: 0)
        }
        
        return elements.removeLast()
    }
    
    mutating func remove(at index: Int) -> Element? {
        guard index < count else {
            return nil
        }
        
        if index == count - 1 {
            return elements.removeLast()
        } else {
            elements.swapAt(index, count - 1)
            
            defer {
                siftUp(from: index)
                siftDown(from: index)
            }
            
            return elements.removeLast()
        }
    }
}

struct PriorityQueue<Element> {

    var elements: Heap<Element>
    
    init(sort: @escaping (Element, Element) -> Bool, elements: [Element] = []) {
        self.elements = Heap<Element>(sort: sort, elements: elements)
    }

    mutating func enqueue(_ element: Element) {
        elements.insert(element)
    }
    
    mutating func dequeue() -> Element? {
        return elements.remove()
    }
}


enum EdgeType {
    case directed
    case undirected
}

protocol Graph {
    associatedtype Element
    func createVertex(data: Element) -> Vertex<Element>
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?)
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?)
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double?
    func edges(from source: Vertex<Element>) -> [Edge<Element>]
}

extension Graph {
    func add(_ edge: EdgeType, from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        switch edge {
        case .directed:
            addDirectedEdge(from: source, to: destination, weight: weight)
        case .undirected:
            addUndirectedEdge(between: source, and: destination, weight: weight)
        }
    }
    
    func addUndirectedEdge(between source: Vertex<Element>, and destination: Vertex<Element>, weight: Double?) {
        addDirectedEdge(from: source, to: destination, weight: weight)
        addDirectedEdge(from: destination, to: source, weight: weight)
    }
}

struct Vertex<Element> {
    let index: Int
    let data: Element
}

extension Vertex: Equatable where Element: Equatable {}
extension Vertex: Hashable where Element: Hashable {}

extension Vertex: CustomStringConvertible {
    var description: String {
        return "\(data)"
    }
}

struct Edge<Element> {
    let source: Vertex<Element>
    let destination: Vertex<Element>
    let weight: Double?
}

class AdjacencyList<Element: Hashable>: Graph {
    var adjacencies: [Vertex<Element>: [Edge<Element>]] = [:]
    
    func createVertex(data: Element) -> Vertex<Element> {
        let vertex = Vertex(index: adjacencies.count, data: data)
        adjacencies[vertex] = []
        return vertex
    }
    
    func addDirectedEdge(from source: Vertex<Element>, to destination: Vertex<Element>, weight: Double?) {
        let edge = Edge(source: source, destination: destination, weight: weight)
        adjacencies[source]?.append(edge)
    }
    
    func weight(from source: Vertex<Element>, to destination: Vertex<Element>) -> Double? {
        return edges(from: source).first { $0.destination == destination }?.weight
    }
    
    func edges(from source: Vertex<Element>) -> [Edge<Element>] {
        return adjacencies[source] ?? []
    }
    
    var vertices: [Vertex<Element>] {
        return Array(adjacencies.keys)
    }
    
    func copyVertices(from graph: AdjacencyList) {
        for vertex in graph.vertices {
            adjacencies[vertex] = []
        }
    }
}

extension AdjacencyList: CustomStringConvertible {
  public var description: String {
    var result = ""
    for (vertex, edges) in adjacencies { // 1
      var edgeString = ""
      for (index, edge) in edges.enumerated() { // 2
        if index != edges.count - 1 {
          edgeString.append("\(edge.destination), ")
} else {
          edgeString.append("\(edge.destination)")
        }
}
      result.append("\(vertex) ---> [ \(edgeString) ]\n") // 3
    }
    return result
  }
}

class Prim<T: Hashable> {
    typealias Graph = AdjacencyList<T>
}

extension Prim {
    func addAvailableEdges(from vertex: Vertex<T>, in graph: Graph, check visited: Set<Vertex<T>>, to priorityQueue: inout PriorityQueue<Edge<T>>) {
        for edge in graph.edges(from: vertex) {
            if !visited.contains(edge.destination) {
                priorityQueue.enqueue(edge)
            }
        }
    }
    
    func produceMinimumSpanningTree(for graph: Graph) -> (cost: Double, mst: Graph) {
        var cost = 0.0
        let mst = Graph()
        var visited: Set<Vertex<T>> = []
        var priorityQueue = PriorityQueue<Edge<T>>(sort: {
            $0.weight ?? 0.0 < $1.weight ?? 0.0
        })
        
        mst.copyVertices(from: graph)
        
        guard let start = graph.vertices.first else {
            return (cost: cost, mst: mst)
        }
        
        visited.insert(start)
        addAvailableEdges(from: start, in: graph, check: visited, to: &priorityQueue)
        
        while let edge = priorityQueue.dequeue() {
            let vertex = edge.destination
            guard !visited.contains(vertex) else {
                continue
            }
            
            cost += edge.weight ?? 0.0
            visited.insert(vertex)
            
            mst.add(.undirected, from: edge.source, to: edge.destination, weight: edge.weight)
            addAvailableEdges(from: vertex, in: graph, check: visited, to: &priorityQueue)
        }
        return (cost: cost, mst: mst)
    }
}

var graph = AdjacencyList<Int>()
let one = graph.createVertex(data: 1)
let two = graph.createVertex(data: 2)
let three = graph.createVertex(data: 3)
let four = graph.createVertex(data: 4)
let five = graph.createVertex(data: 5)
let six = graph.createVertex(data: 6)

graph.add(.undirected, from: one, to: two, weight: 6)
graph.add(.undirected, from: one, to: three, weight: 1)
graph.add(.undirected, from: one, to: four, weight: 5)
graph.add(.undirected, from: two, to: three, weight: 5)
graph.add(.undirected, from: two, to: five, weight: 3)
graph.add(.undirected, from: three, to: four, weight: 5)
graph.add(.undirected, from: three, to: five, weight: 6)
graph.add(.undirected, from: three, to: six, weight: 4)
graph.add(.undirected, from: four, to: six, weight: 2)
graph.add(.undirected, from: five, to: six, weight: 6)

let (cost,mst) = Prim().produceMinimumSpanningTree(for: graph)
print("cost: \(cost)")
print("mst:")
print(mst)

```



## Bucket Sort

```swift
extension Array where Element == Int {
    func bucketSort() -> [Element] {
        guard count > 0 else { return self }
        guard let maxElement = self.max() else { return self }

        var buckets = [[Element]](repeating: [], count: maxElement / 10 + 1)
        var sortedArray = [Element]()

        for num in self {
            buckets[num / 10].append(num)
        }

        sortedArray = buckets.flatMap { $0.sorted() } // typically insertion sort is used with bucket sort. the sort in the standard library is intra sort which uses insertion sort when the input size is small.
        return sortedArray
    }
}

var a = [1, 1, 5, 5, 5, 6, 10, 5, 7, 12, 32]
a.bucketSort()

```

A common optimization is to put the unsorted elements of the buckets back in the original array first, then run insertion sort over the complete array; because insertion sort's runtime is based on how far each element is from its final position, the number of comparisons remains relatively small, and the memory hierarchy is better exploited by storing the list contiguously in memory.

```swift
extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
    mutating func insertionSort() {
        insertionSort(by: <)
    }
    
    mutating func insertionSort(by areInIncreasingOrder: (Element, Element) throws -> Bool) rethrows {
        guard count > 1 else { return }
        
        for currentIndex in index(after: startIndex)..<endIndex {
            for shiftingIndex in (index(after: startIndex)...currentIndex).reversed() {
                let previousIndex = index(before: shiftingIndex)
                if try areInIncreasingOrder(self[shiftingIndex], self[previousIndex]) {
                    swapAt(shiftingIndex, previousIndex)
                } else {
                    break
                }
            }
        }
    }
}

var a = [1, 4, 7, 8, 7, 6, 4, 3, 9, 8, 4, 3, 2, 6, 5, 4]
a.insertionSort()

extension Array where Element == Int {
    mutating func bucketSort() {
        guard count > 0 else { return }
        guard let maxElement = self.max() else { return }

        var buckets = [[Element]](repeating: [], count: maxElement / 10 + 1)

        for num in self {
            buckets[num / 10].append(num)
        }

        self = buckets.flatMap { $0 }

        self.insertionSort(by: <)
    }
}

var b = [100, 400, 10, 2]
b.bucketSort()
```


























## Flood Fill

```swift
class Solution {
    func floodFill(_ image: [[Int]], _ sr: Int, _ sc: Int, _ newColor: Int) -> [[Int]] {
        let color = image[sr][sc]
        guard color != newColor else {
            return image
        }
        
        var result = image
        depthFirstSearch(&result, sr, sc, newColor, color)
        return result
    }
    
    func depthFirstSearch(_ image: inout [[Int]], _ r: Int, _ c: Int, _ newColor: Int, _ oldColor: Int) {
        guard image[r][c] == oldColor else {
            return
        }
        
        image[r][c] = newColor
        if r + 1 < image.count {
            depthFirstSearch(&image, r + 1, c, newColor, oldColor)
        }
        
        if r - 1 >= 0 {
            depthFirstSearch(&image, r - 1, c, newColor, oldColor)
        }
        
        if c + 1 < image[0].count {
            depthFirstSearch(&image, r, c + 1, newColor, oldColor)
        }
        
        if c - 1 >= 0 {
            depthFirstSearch(&image, r, c - 1, newColor, oldColor)
        }
    }
}
```










### Algorithms:

```swift

let swiftDataStructures = [
  "Array",
  "Set",
  "Dictionary",
  "Range",
  "Tuple",
  "LinkedList",
  "Stack",
  "Queue",
  "Tree",
  "Binary Tree",
  "Binary Search Tree",
  "AVL Tree"
]

let haskellDataStructures = [
  "List",
  "Association List",
  "Map"
]

let swiftAlgorithms = [
  "Binary Search - Iterative",
  "Binary Search - Recursive",
  "Bubble Sort",
  "Fibonnaci - Iterative",
  "Fibonnaci - Recursive",
  "Find of perfect squares between two numbers",
  "FizzBuzz",
  "Insertion Sort",
  "Jewels and Stones - (Leetcode)",
  "Merge Sort",
  "Move Zeroes - (Leetcode)",
  "Quick Sort",
  "Radix Sort",
  "Remove Duplicates from Sorted Array - (Leetcode)",
  "Roman to Integer - (Leetcode)",
  "Selection Sort",
  "Shuffle Array - (Austin/Facebook)",
  "Two Sum - Sorted - (Leetcode)",
  "Two Sum - Unsorted - (Leetcode)",
  "Reverse Linked List - Iterative",
  "Reverse Linked List - Recursive",
  "Maximum Subarray - Kadane's Algorithm",
  "Pow(x, n)",
  "Factorial",
  "Create a function that prints out the elements of a linked list in reverse order",
  "Create a function that returns the middle node of a linked list",
  "Create a function that takes two sorted linked lists and merges them into a single sorted linked list",
  "Delete node in a linked list",
  "Remove linked list elements",
  "Remove Duplicates from Sorted List",
  "Intersection of two linked lists",
  "Linked list cycle",
  "Palindrome linked list",
  "Remove Nth Node From End of List",
  "Add Two Numbers - (Leetcode) this is a linked list problem",
  "Remove Duplicates from Sorted List II",
  "Challenge 1: Are the letters unique?",
  "Challenge 2: Is a string a palindrome?",
  "Challenge 3: Do two strings contain the same characters?",
  "Challenge 5: Count the characters",
  "Challenge 6: Remove duplicate letters from a string",
  "Challenge 7: Condense whitespace",
  "Challenge 8: String is rotated",
  "Challenge 9: Find pangrams",
  "Challenge 10: Vowels and consonants"
]

let haskellAlgorithms = [
  "perfect squares between 1 and upper bound",
  "factorial",
  "fibonacci",
  "head",
  "length - list comprehension",
  "length - recursive",
  "sum",
  "maximum - recursive",
  "maximum - with max function",
  "replicate",
  "take",
  "reverse",
  "repeat",
  "zip",
  "elem",
  "quickSort - list comprehension",
  "quickSort - filter",
  "quickSort - partition",
  "zipWith",
  "flip",
  "map",
  "filter",
  "find the sum of all odd squares that are smaller than 10,000",
  "takeWhile",
  "collatz sequence",
  "for all starting numbers between 1 and 100, how many Collatz Sequences have a length greater than 15?"
]

import Darwin

extension Collection where Index == Int {
  func randomElements(to number: Int) -> [Element] {
    var result: [Element] = []
    var index: [Int: Int] = [:]
    while result.count < number {
      let randomIndex = Int(arc4random_uniform(UInt32(endIndex)))
      if index[randomIndex] == nil {
        let randomElement = self[randomIndex]
        index[randomIndex] = randomIndex
        result.append(randomElement)
      }
    }
    return result
  }
}

func problems() {
  print("-----Swift Data Structures to Implement-----")
  print(swiftDataStructures.randomElements(to: 1).first!)
  print("\n")
  
  print("-----Haskell Data Structures to Practice-----")
  print(haskellDataStructures.randomElements(to: 1).first!)
  print("\n")
  
  print("-----Swift Algorithms to Practice-----")
  let swiftAlgorithmList = swiftAlgorithms.randomElements(to: 5)
  swiftAlgorithmList.forEach { print($0) }
  print("\n")
  
  print("-----Haskell Algorithms to Practice-----")
  let haskellAlgorithmList = haskellAlgorithms.randomElements(to: 5)
  haskellAlgorithmList.forEach { print($0) }
  print("\n")
}

problems()


```


extension Int {
  static func random(in range: Range<Int>) -> Int {
    let offset = abs(range.lowerBound)
    let min = UInt32(range.lowerBound + offset)
    let max = UInt32(range.upperBound + offset)
    return Int(min + arc4random_uniform(max - min)) - offset
  }
}


## number of jewels
```swift

class Solution {
  func numJewelsInStones(_ J: String, _ S: String) -> Int {
    let jewels = S.filter { J.contains($0) }
    return jewels.count
  }
}



//why is unicode scalar solution faster?

class Solution {
    func numJewelsInStones(_ J: String, _ S: String) -> Int {
        return S.unicodeScalars.filter { J.unicodeScalars.contains($0) }.count
    }
}

extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    var count = 0
    for element in self {
      if predicate(element) {
        count += 1
      }
    }
    return count
  }
}

class Solution {
  func numJewelsInStones<T: Sequence>(_ J: T, _ S: T) -> Int where T.Iterator.Element: Equatable  {
    return S.count(where: { J.contains($0) })
  }
}

extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return filter { predicate($0) }.count
  }
}

class Solution {
  func numJewelsInStones<T: Sequence>(_ J: T, _ S: T) -> Int where T.Iterator.Element: Equatable  {
    return S.count(where: { J.contains($0) })
  }
}

Solution().numJewelsInStones("aA", "aAAbbbb")


extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return filter { predicate($0) }.count
  }
}

class Solution {
  func numJewelsInStones<T: Sequence>(_ J: T, _ S: T) -> Int where T.Iterator.Element: Equatable  {
    return S.count(where: J.contains)
  }
}

Solution().numJewelsInStones("aA", "aAAbbbb")


Time Complexity: O(J\text{.length} + S\text{.length}))O(J.length+S.length)). The O(J\text{.length})O(J.length) part comes from creating J. The O(S\text{.length})O(S.length) part comes from searching S.

Space Complexity: O(J\text{.length})O(J.length).



extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return filter { predicate($0) }.count
  }
}

class Solution {
  func numJewelsInStones(_ J: String, _ S: String) -> Int {
    let jSet = Set(J)
    return S.count(where: jSet.contains)
  }
}

// using a set will be faster because the init is O(n) but subseqent contains calls will be O(1)
// where using an array, each contains call will be O(n)


```



two sum sorted -- two pointer approach 
time O(n)
space - O(1) because it's always two pointers

two sum unsorted -- hash approach
time O(n)
space - O(n) becasue of the hash table

if we had an unsorted array and wanted to optimize for space and not speed then we would do an inplace sort then we would do the two pointer approach.





while a true inplace sort is not possible in swift, making a new array could be a quick operation
especially if we set reservecapacity so it didn't have to keep making new arrays assuming we know the input size. We don't need to rely on geometric growth here.




## roman to int 
```swift 
class Solution {
  func romanToInt(_ s: String) -> Int {
    var accumulator = 0

    for index in 0..<s.count {
      var romanValue = value(for: s[index])

      let nextIndex = index + 1
      if nextIndex < s.count, value(for: s[nextIndex]) > value(for: s[index]) {
        romanValue *= -1
      }
      accumulator += romanValue
    }
    return accumulator
  }

  private static let romanValues: [Character: Int] = ["I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000]

  private func value(for character: Character) -> Int {
    return Solution.romanValues[character] ?? 0
  }
}

extension String {
  subscript (offset: Int) -> Character {
    return self[index(startIndex, offsetBy: offset)]
  }
}

Solution().romanToInt("XXVIIIXDDM")

// Switch statement is faster than a dictionary lookup
// Switch is faster because of wrapping and unwrapping values from the dictionary slows it down
// switches can also be faster based on how the compile treats it. For example, switches can be made with jump tables or even binary searchs or a long series of if statements. There are no gauratees. Switches can be faster than dictionaries, but dictionaries are always gaurantees a constant time lookup and they are easier to maintain therefore are the preferred choice
```

# Completed

https://www.youtube.com/watch?v=p3vVYNngyxs


https://www.youtube.com/watch?v=pDyh9VOMWgI




https://stackoverflow.com/questions/22666535/shared-ancestor-between-two-views


https://medium.com/journey-of-one-thousand-apps/finding-the-first-common-superview-in-swift-4abac8a87d84
https://medium.com/journey-of-one-thousand-apps/data-structures-35b968bcedf5
https://medium.com/journey-of-one-thousand-apps/building-with-python-requests-d9260b26e7ab

https://medium.com/journey-of-one-thousand-apps/building-a-hash-data-structure-in-swift-e9b2733d9e20

https://medium.com/journey-of-one-thousand-apps/data-structures-in-the-real-world-508f5968545a

https://medium.com/journey-of-one-thousand-apps/background-thread-and-main-dispatch-50f19b000d8
https://medium.com/journey-of-one-thousand-apps/testing-asynchronous-code-in-swift-2142901dbd4f
https://medium.com/journey-of-one-thousand-apps/complexity-and-big-o-notation-in-swift-478a67ba20e7
https://stackoverflow.com/questions/43230994/common-parent-view-in-swift




https://www.youtube.com/user/tusharroy2525/videos




## Implement strStr()

Knuth–Morris–Pratt(KMP) Pattern Matching(Substring search)
https://www.youtube.com/watch?v=GTJr8OvyEVQ
// O(m + n)
m = length of haystack
n = length of needle






https://www.youtube.com/watch?v=WIoZuhAC1p4


##two sum

extension Collection where Element == Int {
  func twoSum(for target: Element) -> (Element, Element)? {
    guard count > 1 else { return nil }
    
    var elementsSeen = Set<Int>()
    elementsSeen.reserveCapacity(Int(count))
    
    for element in self {
      print(elementsSeen)
      let compliment = target - element
      if elementsSeen.contains(compliment) {
        return (compliment, element)
      } else {
        elementsSeen.insert(element)
      }
    }
    
    return nil
  }
}

var a = [1, 5, 4, 8, 66, 4, 12, 3, 2, 1, 6, 7, 8, 99, 0]
print(a.twoSum(for: 100))

we can reserve capacity because we don't need a geometric growth strategy. we already know the maximum amount of numbers possible is equal to the count of the collection so we can just reserve capacity for the size of the collection




## Add Two Numbers
// Time complexity : O(\max(m, n))O(max(m,n)). Assume that mm and nn represents the length of l1l1 and l2l2 respectively, the algorithm above iterates at most \max(m, n)max(m,n) times.
// Space complexity : O(\max(m, n))O(max(m,n)). The length of the new list is at most \max(m,n) + 1max(m,n)+1.
```swift
func addTwoNumbers(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
  var currentL1Node = l1
  var currentL2Node = l2
  
  let head = ListNode(0)
  var current = head
  var carry = 0
  
  while currentL1Node != nil || currentL2Node != nil {
    let l1NodeValue = currentL1Node?.val ?? 0
    let l2NodeValue = currentL2Node?.val ?? 0

    let total = l1NodeValue + l2NodeValue + carry
    carry = total / 10
    current.next = ListNode(total % 10)
    current = current.next!
    currentL1Node = currentL1Node?.next
    currentL2Node = currentL2Node?.next
  }
  
  if carry > 0 {
    current.next = ListNode(carry)
  }
  return head.next
}
```


## Bubble Sort
Best - O(n)
Worst - O(n^2)
Space - O(1)

```swift
extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  mutating func bubbleSort() {
    guard count > 1 else { return }
    
    for endingIndex in indices.reversed() {
      var noSwaps = true
      for currentIndex in startIndex..<endingIndex {
        let nextIndex = index(after: currentIndex)
        if self[currentIndex] > self[nextIndex] {
          swapAt(currentIndex, nextIndex)
          noSwaps = false
        }
      }
      if noSwaps { return }
    }
  }
}

var a = [6, 3, 8, 7, 5, 4, 2, 9, 8, 5, 4, 2]
a.bubbleSort()

extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  mutating func bubbleSort(by sort: (Element, Element) -> Bool = (<)) {
    guard count > 1 else { return }
    
    for endingIndex in indices.reversed() {
      var noSwaps = true
      for currentIndex in startIndex..<endingIndex {
        let nextIndex = index(after: currentIndex)
        if sort(self[nextIndex], self[currentIndex]) {
          swapAt(currentIndex, nextIndex)
          noSwaps = false
        }
      }
      if noSwaps { return }
    }
  }
}

var a = [5, 4, 5, 4, 5, 6, 5, 4, 3, 4, 3, 2, 3, 2, 1, 1]
a.bubbleSort(by: <)
```


## Insertion Sort
Best - O(n)
Worst - O(n^2)
Space - O(1)

```swift
extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  mutating func insertionSort() {
    guard count > 1 else { return }
    
    for currentIndex in index(after: startIndex)..<endIndex {
      for shiftingIndex in (index(after: startIndex)...currentIndex).reversed() {
        let previousIndex = index(before: shiftingIndex)
        if self[previousIndex] > self[shiftingIndex] {
          swapAt(previousIndex, shiftingIndex)
        } else {
          break
        }
      }
    }
  }
}

var b = [6, 3, 8, 7, 5, 4, 2, 9, 8, 5, 4, 2]
b.insertionSort()

extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  mutating func insertionSort(by sort: (Element, Element) -> Bool = (<)) {
    guard count > 1 else { return }
    
    for currentIndex in index(after: startIndex)..<endIndex {
      for shiftingIndex in (index(after: startIndex)...currentIndex).reversed() {
        let previousIndex = index(before: shiftingIndex)
        if sort(self[shiftingIndex], self[previousIndex]) {
          swapAt(shiftingIndex, previousIndex)
        } else {
          break
        }
      }
    }
  }
}

var a = [5, 4, 6, 5, 4, 5, 4, 3, 4, 3, 2, 1]
a.insertionSort()
```

## Selection Sort
Best - O(n^2)
Worst - O(n^2)
Space - O(1)

```swift
extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  mutating func selectionSort() {
    guard count > 1 else { return }
    
    for currentIndex in startIndex..<index(before: endIndex) {
      var lowestIndex = currentIndex
      for swapIndex in index(after: currentIndex)..<endIndex {
        if self[swapIndex] < self[lowestIndex] {
          lowestIndex = swapIndex
        }
      }
      swapAt(lowestIndex, currentIndex)
    }
  }
}

var c = [6, 3, 8, 7, 5, 4, 2, 9, 8, 5, 4, 2]
c.selectionSort()

extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  mutating func selectionSort(_ sort: (Element, Element) -> Bool = (<)) {
    guard count > 1 else { return }
    
    for currentIndex in startIndex..<index(before: endIndex) {
      var lowestValueIndex = currentIndex
      for swapIndex in index(after: currentIndex)..<endIndex {
        if sort(self[swapIndex], self[lowestValueIndex]) {
          lowestValueIndex = swapIndex
        }
      }
      swapAt(lowestValueIndex, currentIndex)
    }
  }
}

var a = [9, 8, 7, 8, 7, 6, 7, 6, 5, 4, 4, 4, 3, 2, 1, 2, 3, 2]
a.selectionSort()
```

## Merge Sort
Best - O(n log n)
Worst - O(n log n)
Space - O(n log n)

```swift
public func mergeSort<Element: Comparable>(_ array: [Element]) -> [Element] {
  guard array.count > 1 else { return array }
  let middleIndex = array.count / 2
  let left = mergeSort(Array(array[..<middleIndex]))
  let right = mergeSort(Array(array[middleIndex...]))
  return merge(left, right)
}

private func merge<Element: Comparable>(_ left: [Element], _ right: [Element]) -> [Element] {
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
  }
  
  if right.count > rightIndex {
    result.append(contentsOf: right[rightIndex...])
  }
  
  return result
}

var d = [6, 3, 8, 7, 5, 4, 2, 9, 8, 5, 4, 2]
mergeSort(d)

extension RandomAccessCollection where Element: Comparable {
  public func mergeSort(by sort: (Element, Element) -> Bool = (<)) -> [Element] {
    guard count > 1 else { return Array(self) }
    let middleIndex = index(startIndex, offsetBy: count / 2)
    let left = Array(self[..<middleIndex]).mergeSort()
    let right = Array(self[middleIndex...]).mergeSort()
    return merge(left, right, by: sort)
  }
  
  private func merge(_ left: [Element], _ right: [Element], by sort: (Element, Element) -> Bool = (<)) -> [Element] {
    var results: [Element] = []
    var leftIndex = 0
    var rightIndex = 0
    
    while left.count > leftIndex && right.count > rightIndex {
      let leftElement = left[leftIndex]
      let rightElement = right[rightIndex]
      
      if leftElement == rightElement {
        results.append(leftElement)
        results.append(rightElement)
        leftIndex += 1
        rightIndex += 1
      } else if sort(leftElement, rightElement) {
        results.append(leftElement)
        leftIndex += 1
      } else {
        results.append(rightElement)
        rightIndex += 1
      }
    }
    
    if left.count > leftIndex {
      results.append(contentsOf: left[leftIndex...])
    } else {
      results.append(contentsOf: right[rightIndex...])
    }
    
    return results
  }
}

var a = [9, 8, 7, 8, 7, 6, 7, 6, 5, 6, 5, 4, 5, 4, 3, 2, 1]
a.mergeSort(by: >)
// need to fix the predicate for ordering greatest to least
```

## Radix Sort
Best - O(nk)
Worst - O(nk)
Space - O(n + k)

```swift
extension Array where Element == Int {
  mutating func radixSort() {
    guard count > 1 else { return }
    
    var done = false
    var digits = 1
    let radix = 10
    
    while !done {
      done = true
      var buckets: [[Int]] = .init(repeating: [], count: radix)
      
      for number in self {
        let remainingPart = number / digits
        let digit = remainingPart % radix
        buckets[digit].append(number)
        
        if remainingPart > 0 {
          done = false
        }
      }
      
      self = buckets.flatMap { $0 }
      digits *= 10
    }
  }
}

var a = [1, 4, 3, 5, 6, 7, 8, 5, 4, 3, 2, 8, 7]
a.radixSort()
```

## Heap Sort
Best - O(n log n)
Worst - O(n log n)
Space - O(1)

```swift
extension Array where Element: Comparable {
  func leftChildIndex(ofParentAt index: Int) -> Int {
    return 2 * index + 1
  }
  
  func rightChildIndex(ofParentAt index: Int) -> Int {
    return 2 * index + 2
  }
  
  mutating func siftDown(from index: Int, upTo size: Int) {
    var parent = index
    while true {
      let left = leftChildIndex(ofParentAt: parent)
      let right = rightChildIndex(ofParentAt: parent)
      var candidate = parent
      
      if left < size && (self[left] > self[candidate]) {
        candidate = left
      }
      
      if right < size && (self[right] > self[candidate]) {
        candidate = right
      }
      
      if parent == candidate {
        return
      }
      
      swapAt(parent, candidate)
      parent = candidate
      
    }
  }
  
  mutating func heapSort() {
    guard count > 1 else { return }
    
    for i in stride(from: count / 2 - 1, through: 0, by: -1) {
      siftDown(from: i, upTo: count)
    }
    
    for index in indices.reversed() {
      swapAt(0, index)
      siftDown(from: 0, upTo: index)
    }
  }
}

var a = [8, 7, 8, 7, 6, 7, 6, 5, 3, 2, 4, 3, 1, 1]
a.heapSort()
```


## Quick Sort
Best - O(n log n)
Worst - O(n^2)
Space - O(1)

https://stackoverflow.com/questions/41031106/which-general-purpose-sorting-algorithm-does-swift-use-it-does-not-perform-well

https://stackoverflow.com/questions/27677026/swift-sorting-algorithm-implementation



```swift
import Foundation

public func quickSort<Element: Comparable>(_ array: inout [Element]) {
  quickSort(&array, array.startIndex, array.index(before: array.endIndex))
}

private func quickSort<Element: Comparable>(_ array: inout [Element], _ low: Int, _ high: Int) {
  guard low < high else { return }
  
  let randomIndex = random(low, high)
  array.swapAt(randomIndex, high)
  
  let partitionIndex = partition(&array, low, high)
  quickSort(&array, low, array.index(before: partitionIndex))
  quickSort(&array, array.index(after: partitionIndex), high)
}

private func partition<Element: Comparable>(_ array: inout [Element], _ low: Int, _ high: Int) -> Int {
  let partitionElement = array[high]
  var lowIndex = low
  
  for currentIndex in low..<high {
    if array[currentIndex] <= partitionElement {
      array.swapAt(currentIndex, lowIndex)
      lowIndex += 1
    }
  }
  
  array.swapAt(lowIndex, high)
  return lowIndex
}

private func random(_ min: Int, _ max: Int) -> Int {
  return min + Int(arc4random_uniform(UInt32(max - min) + 1))
}

var a = [5, 3, 2, 7, 6, 8, 7, 3, 2, 1, 3, 4, 6, 8]
quickSort(&a)
```

## Binary Search (Recursive)
Best - O(log n)
Worst - O(log n)
Space - O(log n)

```swift

```

## Binary Search (Iterative)
Best - O(log n)
Worst - O(log n)
Space - O(1)

```swift

```

## Two Sum (Sorted Array Guaranteed)

```swift
func twoSum(_ array: [Int], _ sum: Int) -> Bool {
  var lowIndex = 0
  var highIndex = array.count - 1
  
  while lowIndex < highIndex {
    let sumOfItems = array[lowIndex] + array[highIndex]
    if sumOfItems == sum {
      return true
    } else if sumOfItems > sum {
      highIndex -= 1
    } else if sumOfItems < sum {
      lowIndex += 1
    }
  }
  return false
}

extension RandomAccessCollection where Element == Int {
  func twoSum(for sum: Int) -> (Int, Int)? {
    guard count > 1 else { return nil }
    
    var left = startIndex
    var right = index(before: endIndex)
    
    while left < right {
      let value = self[left] + self[right]
      
      if value == sum {
        return (self[left], self[right])
      } else if value > sum {
        right = index(before: right)
      } else {
        left = index(after: left)
      }
    }
    return nil
  }
}
```


## Two Sum (Sorted Array Not Guaranteed)

```swift
func twoSum(_ nums: [Int], _ target: Int) -> [Int] {
  var dict = [Int: Int]()
  
  for (index, num) in nums.enumerated() {
    let complement = target - num
    
    if let complementIndex = dict[complement] {
      return [complementIndex, index]
    }
    
    dict[num] = index
  }
  return []
}

extension RandomAccessCollection where Element == Int {
  func twoSum(for target: Int) -> (Int, Int)? {
    guard count > 1 else { return nil }
    var numbersSeen = Set<Int>()
    
    for num in self {
      let complement = target - num
      if numbersSeen.contains(complement) {
        return (complement, num)
      } else {
        numbersSeen.insert(num)
      }
    }
    
    return nil
  }
}
```

## Two Sum - Input is a Binary Search Tree


## FizzBuzz
```swift
struct FizzBuzz: Sequence, IteratorProtocol {
  var count = 1
  
  mutating func next() -> String? {
    defer { count += 1 }
    return value(for: count)
  }
  
  private func value(for number: Int) -> String {
    if number.divisible(by: 15) { return "FizzBuzz" }
    if number.divisible(by: 3) { return "Fizz" }
    if number.divisible(by: 5) { return "Buzz" }
    return "\(number)"
  }
}

extension Int {
  func divisible(by denominator: Int) -> Bool {
    return self % denominator == 0
  }
}

for value in FizzBuzz().prefix(15) {
  print(value)
}

```


## Fibonacci
```swift
struct Fibonacci: Sequence, IteratorProtocol {
  var state = (0, 1)
  
  mutating func next() -> Int? {
    let upcomingNumber = state.0
    state = (state.1, state.0 + state.1)
    return upcomingNumber
  }
}


for num in Fibonacci().prefix(10) {
  print(num)
}
```

## Most Common Element in an array

```swift
var a = [1, 1, 1, 2, 3, 4, 5, 5, 6, 7, 8, 9, 10, 2, 3, 4, 1, 2, 3, 3, 3, 4, 5]


func mostFrequent<T: Hashable>(array: [T]) -> (value: T, count: Int)? {
  
  let counts = array.reduce(into: [:]) { $0[$1, default: 0] += 1 }
  
  if let (value, count) = counts.max(by: { $0.1 < $1.1 }) {
    return (value, count)
  }
  
  // array was empty
  return nil
}

if let result = mostFrequent(array: a) {
  print("\(result.value) occurs \(result.count) times")
}


```


## Shuffle Array in place









var a = [1, 2, 3, 4]
withUnsafePointer(to: &a) { print("\($0)") }

var b = [1...1000]
withUnsafePointer(to: &b) { print("\($0)") }
var c = [1...1000]
var d = [1...1000]

a.append(contentsOf: 5...500)
withUnsafePointer(to: &b) { print("\($0)") }
a.capacity
withUnsafePointer(to: &a) { print("\($0)") }

https://en.wikipedia.org/wiki/In-place_algorithm#In_functional_programming


### jewels in stones
```swift 
infix operator ~

extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return filter(predicate).count
  }

  func count(where predicate: (Element) -> Bool) -> Int {
    return reduce(0) { predicate($1) ? $0 + 1 : $0 }
  }
  // https://github.com/apple/swift/pull/16099/commits
  why to use reduce over for loop.
  filter vs reduce here?
  reduce is more declarative 

}

extension Sequence where Element: Hashable {
  static func ~ (lhs: Self, rhs: Self) -> Int {
    return rhs.occurences(in: lhs)
  }
  
  func occurences(in other: Self) -> Int {
    let elementSet = Set(self)
    return other.count(where: elementSet.contains)
  }
}

class Solution {
  func numJewelsInStones(_ J: String, _ S: String) -> Int {
    return S ~ J
  }
}
```




struct Alphabet: Collection {
    var startIndex = 0
    var endIndex = 26
    
    private let base = 65
    
    func index(after index: Int) -> Int {
        guard index < endIndex else { fatalError() }
        return index + 1
    }
    
    subscript(offset: Int) -> String {
        guard offset < endIndex,
              let unicode = UnicodeScalar(base + offset) else { fatalError() }
        return String(describing: unicode)
    }
}


let alphabet = Alphabet()
alphabet[25]





postfix operator ~


struct FizzBuzz: Sequence, IteratorProtocol {
    var current = 1
    
    mutating func next() -> String? {
        defer { current += 1 }
        return FizzBuzz.value(for: current)
    }
    
    static func value(for number: Int) -> String {
        if number.divisible(by: 15) { return "FizzBuzz" }
        if number.divisible(by: 3)  { return "Fizz" }
        if number.divisible(by: 5)  { return "Buzz" }
        return "\(number)"
    }
}

extension Int {
    func divisible(by number: Int) -> Bool {
        return self % number == 0
    }
    
    static postfix func ~ (number: Int) -> String {
        return FizzBuzz.value(for: number)
    }
}

for number in 1..<100 {
    print(number~)
}



extension BidirectionalCollection where Element == Int {
    func twoSum(for target: Element) -> (Element, Element)? {
        guard count > 1 else { return nil }
        
        var left = startIndex
        var right = index(before: endIndex)
        
        while left <= right {
            let sum = self[left] + self[right]
            
            if sum == target {
                return (self[left], self[right])
            }
            
            if sum > target {
                right = index(before: right)
            }
            
            if sum < target {
                left = index(after: left)
            }
        }
        return nil
    }
}

var a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
a.twoSum(for: 13)



infix operator ~~

extension Sequence {
    func count(where predicate: (Element) -> Bool) -> Int {
        var count = 0
        for element in self where predicate(element)  {
            count += 1
        }
        return count
    }
}

extension Sequence where Element: Hashable {

    static func ~~ (_ lhs: Self, rhs: Self) -> Int {
        return lhs.occurrences(in: rhs)
    }

    func occurrences(in other: Self) -> Int {
        let elementSet = Set(self)
        return other.count(where: elementSet.contains)
    }
}

class Solution {
    func numJewelsInStones(_ J: String, _ S: String) -> Int {
        return J.unicodeScalars ~~ S.unicodeScalars
    }
}

"Aa" ~~ "AaaaaBBBBCCaaaa"








class Solution {
    func removeDuplicates(_ nums: inout [Int]) -> Int {
        guard nums.count > 1 else { return nums.count }
        
        var i = 0
        
        for j in 1..<nums.endIndex {
            if nums[i] != nums[j] {
                i += 1
                nums[i] = nums[j]
            }
        }
        
        return i + 1
    }
}




    func moveZeroes(_ nums: inout [Int]) {
        var currentIndex = 0
        var previousIndex = 0

        while currentIndex < nums.count {
            if nums[currentIndex] != 0 {
                nums.swapAt(previousIndex, currentIndex)
                previousIndex += 1
            }
            currentIndex += 1
        }
    }





    extension Sequence {
  func map<T>(while predicate: (Element) -> Bool, with transform: (Element) -> T) -> [T] {
    var result = [T]()
    
    for element in self where predicate(element) {
      result.append(transform(element))
    }
    
    return result
  }
}

func perfectSquares(in range: CountableRange<Int>) -> [Int] {
  guard range.count > 0 else { return [] }
  
  return range.map(while: { $0 * $0 < range.upperBound }, with: { $0 * $0 })
}

perfectSquares(in: 1..<20)


// Find of perfect squares between two numbers

extension CountableRange where Bound == Int {
  func perfectSquares() -> [Int] {
    guard lowerBound < upperBound else { return [] }
    
    var squareRoot = lowerBound
    
    return reduce(into: [], { (result, element) in
      let square = squareRoot * squareRoot
      guard square < upperBound else { return }
      result.append(square)
      squareRoot += 1
    })
  }
}




extension RandomAccessCollection where Element: Comparable {
  mutating func binarySearch(for target: Element, in range: Range<Index>? = nil) -> Bool {
    let range = range ?? startIndex..<endIndex
    guard range.lowerBound < range.upperBound else { return false }
    
    let size = distance(from: range.lowerBound, to: range.upperBound)
    let middleIndex = index(range.lowerBound, offsetBy: size / 2)
    let middleValue = self[middleIndex]
    
    if target == middleValue {
      return true
    }
    
    if target > middleValue {
      return binarySearch(for: target, in: index(after: middleIndex)..<range.upperBound)
    }
    
    if target < middleValue {
      return binarySearch(for: target, in: range.lowerBound..<middleIndex)
    }
    
    return false
  }
}

var a = [1, 2, 3, 4, 5, 6, 7, 8, 9]
a.binarySearch(for: 10)



http://www.mczarnik.com/2017/04/08/break-out-of-reduce.html


// array duplicates

extension Sequence where Element: Hashable {
  func unique() -> [Element] {
    var seen: Set<Element> = []
    return filter { element in
      if seen.contains(element) {
        return false
      } else {
        seen.insert(element)
        return true
      }
    }
  }
}

class Solution {
  func removeDuplicates(_ nums: inout [Int]) -> Int {
    nums = nums.unique()
    return nums.count
  }
}

var a = [0,0,1,1,1,2,2,3,3,4]
Solution().removeDuplicates(&a)
print(a)

//Do not allocate extra space for another array, you must do this by modifying the input array in-place with O(1) extra memory. the above solution meets these critera in swift because even if you manipulate points you are still creating a new array as the swapAt function is mutating https://developer.apple.com/documentation/swift/array/2893281-swapat
// but the seen set is an auxiliary data structure therefore this solution would probably not be accepted and you would want to use the pointers instead



//Binary Search - Iterative

extension RandomAccessCollection where Element: Comparable {
  func binarySearch(for key: Element) -> Bool {
    var leftIndex = startIndex
    var rightIndex = index(before: endIndex)
    
    while leftIndex <= rightIndex {
      let size = distance(from: leftIndex, to: rightIndex)
      let middleIndex = index(leftIndex, offsetBy: size / 2)
      let middleValue = self[middleIndex]
      
      if middleValue == key {
        return true
      }
      
      if middleValue > key {
        rightIndex = index(before: middleIndex)
      }
      
      if middleValue < key {
        leftIndex = index(after: middleIndex)
      }
    }
    return false
  }
}

[1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13].binarySearch(for: 0)

## binary search recursive
```swift
//- Binary Search - Recursive

extension RandomAccessCollection where Element: Comparable {
  func binarySearch(for key: Element, in range: Range<Index>? = nil) -> Bool {
    let range = range ?? startIndex..<endIndex
    guard range.lowerBound < range.upperBound else { return false }
    let size = distance(from: range.lowerBound, to: range.upperBound)
    let middleIndex = index(range.lowerBound, offsetBy: size / 2)
    let middleValue = self[middleIndex]
    
    if middleValue == key {
      return true
    }
    
    if middleValue > key {
      return binarySearch(for: key, in: range.lowerBound..<middleIndex)
    }
    
    if middleValue < key {
      return binarySearch(for: key, in: index(after: middleIndex)..<range.upperBound)
    }
    return false
  }
}

var a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
a.binarySearch(for: 1)
```


# Linked List
```swift

class Node<Value> {
  let value: Value
  var next: Node?
  
  init(value: Value, next: Node? = nil) {
    self.value = value
    self.next = next
  }
}

extension Node: CustomStringConvertible {
  public var description: String {
    guard let next = next else {
      return "\(value)"
    }
    return "\(value) -> \(next)"
  }
}

struct LinkedList<Value> {
  var head: Node<Value>?
  var tail: Node<Value>?
  
  init() {}
  
  var isEmpty: Bool {
    return head == nil
  }
}

extension LinkedList: CustomStringConvertible {
  public var description: String {
    guard let head = head else {
      return ""
    }
    return "\(head)"
  }
}

extension LinkedList {
  mutating func push(_ value: Value) {
    head = Node(value: value, next: head)
    if tail == nil {
      tail = head
    }
  }
  
  mutating func append(_ value: Value) {
    guard !isEmpty else {
      push(value)
      return
    }
    tail?.next = Node(value: value)
    tail = tail?.next
  }
  
  @discardableResult
  mutating func insert(_ value: Value, after node: Node<Value>) -> Node<Value>? {
    guard tail !== node else {
      append(value)
      return tail
    }
    node.next = Node(value: value, next: node.next)
    return node.next
  }
  
  func node(at index: Int) -> Node<Value>? {
    var currentNode = head
    var currentIndex = 0
    
    while currentNode != nil && currentIndex < index {
      currentNode = currentNode?.next
      currentIndex += 1
    }
    return currentNode
  }
}

extension LinkedList {
  @discardableResult
  mutating func pop() -> Value? {
    defer {
      head = head?.next
      if isEmpty {
        tail = nil
      }
    }
    return head?.value
  }
  
  @discardableResult
  mutating func removeLast() -> Value? {
    guard let head = head else {
      return nil
    }
    
    guard head.next != nil else {
      return pop()
    }
    
    var current = head
    var previous = head
    
    while let next = current.next {
      previous = current
      current = next
    }
    
    previous.next = nil
    tail = previous
    return current.value
  }
  
  @discardableResult
  mutating func remove(after node: Node<Value>) -> Value? {
    defer {
      if tail === node.next {
        tail = node
      }
      node.next = node.next?.next
    }
    return node.next?.value
  }
}


```



```swift
// reverse a linked list iterative

func reverseList<Value>(_ head: Node<Value>?) -> Node<Value>? {
  var currentNode = head
  var previous: Node<Value>?
  var next: Node<Value>?
  
  while currentNode != nil {
    next = currentNode?.next
    currentNode?.next = previous
    previous = currentNode
    currentNode = next
  }
  return previous
}


```



## perfect squares

//Find of perfect squares between two numbers
// O(log n)
// O(1)


postfix operator **

extension Int {
  static postfix func **(number: Int) -> Int {
    return number * number
  }
}

extension CountableRange where Bound == Int {
  func perfectSquares() -> [Bound] {
    var result = [Bound]()
    
    for number in self where number** < upperBound {
      result.append(number**)
    }
    
    return result
  }
}

(1..<100).perfectSquares()



https://www.youtube.com/watch?v=MRe3UsRadKw





```swift
//Two Sum - Sorted - (Leetcode)
// Time O(n)
// Space O(1)

extension RandomAccessCollection where Element == Int {
  func twoSum(for target: Element) -> (Index, Index)? {
    guard count > 1 else { return nil }
    
    var left = startIndex
    var right = index(before: endIndex)

    while left < right {
      let total = self[left] + self[right]
      
      if total == target {
        return (left, right)
      }
      
      if total > target {
        right = index(before: right)
      }
      
      if total < target {
        left = index(after: left)
      }
    }
    return nil
  }
}

var a = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
a.twoSum(for: 2)
```






//Reverse Linked List - Recursive
// Time O(n)
// Space O(n)

class Node<Value> {
  let value: Value
  var next: Node<Value>?
  
  init(_ value: Value, next: Node<Value>? = nil) {
    self.value = value
    self.next = next
  }
}

extension Node: CustomStringConvertible {
  public var description: String {
    guard let next = next else {
      return "\(value)"
    }
    return "\(value) -> \(next)"
  }
}

let node1 = Node(1)
let node2 = Node(2)
let node3 = Node(3)
let node4 = Node(4)

node1.next = node2
node2.next = node3
node3.next = node4

func reverseRecursive<Value>(_ node: Node<Value>?) -> Node<Value>? {
  guard let node = node else { return nil }
  guard node.next != nil else { return node }
  let head = reverseRecursive(node.next)
  node.next?.next = node
  node.next = nil
  return head
}

print(node1)
//print(reverseRecursive(node1))


//Reverse Linked List - Iter
// Time O(n)
// Space O(1)

func reverseIterative<Value>(_ node: Node<Value>?) -> Node<Value>? {
  var current = node
  var previous: Node<Value>? = nil
  var next: Node<Value>? = nil
  
  while current != nil {
    next = current?.next
    current?.next = previous
    previous = current
    current = next
  }
  return previous
}

print(reverseIterative(node1))







//Roman to Integer - (Leetcode)
// T O(n)
// S O(1)

let romanValues: [Character: Int] = ["I": 1, "V": 5, "X": 10, "L": 50, "C": 100, "D": 500, "M": 1000]

class Solution {
  func romanToInt(_ s: String) -> Int {
    var accumulator = 0
    
    for index in s.indices {
      var romanValue = value(for: s[index])
      
      let nextIndex = s.index(after: index)
      if nextIndex < s.endIndex, value(for: s[nextIndex]) > romanValue {
        romanValue *= -1
      }

      accumulator += romanValue
    }
    
    return accumulator
  }
  
  func value(for character: Character) -> Int {
    return romanValues[character] ?? 0
  }
}

Solution().romanToInt("XXIV")







//Move Zeroes - (Leetcode)
// T O(n)
// S O(1)

class Solution {
  func moveZeroes(_ nums: inout [Int]) {
    var i = 0
    
    for j in 1..<nums.count {
      if nums[i] != nums[j] {
        nums.swapAt(i, j)
        i += 1
      }
    }
  }
}

var a = [0,1,0,3,12]
Solution().moveZeroes(&a)











class Solution {
  func maxSubArray(_ nums: [Int]) -> Int {
    guard nums.count > 0 else { return 0 }
    guard nums.count > 1 else { return nums[0] }
    
    var currentMax = Int.min
    var globalMax = Int.min
    
    for num in nums {
      currentMax = num + max(currentMax, 0)
      globalMax = max(currentMax, globalMax)
    }
    return globalMax
  }
}


Solution().maxSubArray([-2,1,-3,4,-1,2,1,-5,4])


# Fib recursive
func fib(_ num: Int) -> Int {
  guard num > 1 else { return num }
  return fib(num - 1) + fib(num - 2)
}





struct UnfoldingSequence<State, T>: Sequence, IteratorProtocol {
  var state: State
  let _next: (inout State) -> T?
  
  init(state: State, next: @escaping (inout State) -> T?) {
    self.state = state
    _next = next
  }
  
  mutating func next() -> T? {
    print(state)
    return _next(&state)
  }
}

let fibs = UnfoldingSequence(state: (0, 1)) { (state) -> Int? in
  defer { state = (state.1, state.0 + state.1) }
  return state.0
}

let r = AnySequence(fibs.prefix(10).map { $0 * 2 })

//Fibonnaci - Iterative

let fibs = sequence(first: (0, 1)) { ($1, $0 + $1) }
for fib in fibs.prefix(10) {
  print(fib.0)
}

https://github.com/raywenderlich/swift-algorithm-club/tree/master/Combinatorics

Time - O(n)
Space - O(n)

func factorialR(_ num: Int) -> Int {
  guard num > 1 else { return 1 }
  return num * factorialR(num - 1)
}

Time - O(n)
Space - O(1)

func factorialI(_ num: Int) -> Int {
  guard num > 1 else { return 1 }
  
  var num = num
  var result = 1
  
  while num > 1 {
    result *= num
    num -= 1
  }
  return result
}

factorialR(3)


var fibonacciArray:[Int] = [0,1]

var fibonacciTest = 6

func fibonacci(_ n: Int) -> Int {
  if (fibonacciArray.count > n) {
    return fibonacciArray[n]
  }
  
  let fibonacciNumber = fibonacci(n - 1) + fibonacci(n - 2)
  fibonacciArray.append(fibonacciNumber)
  return fibonacciNumber
}

fibonacci(fibonacciTest)




extension CountableRange where Bound == Int {
  func perfectSquares() -> [Bound] {
    var result = [Bound]()
    
    var current = lowerBound
    
    while current * current < upperBound {
      result.append(current * current)
      current += 1
    }
    
    return result
  }
}

(0..<100).perfectSquares()








challenge 6
extension Sequence where Element: Hashable {
  func unique() -> [Element] {
    var seenElements = Set<Element>()
    return filter { element in
      if seenElements.contains(element) {
        return false
      } else {
        seenElements.insert(element)
        return true
      }
    }
  }
}

extension String {
  func removeDuplicates() -> String {
    return String(unique())
  }
}

"Hello".removeDuplicates()






// Linked List

class Node<Value> {
  let value: Value
  var next: Node<Value>?
  
  init(_ value: Value, next: Node<Value>? = nil) {
    self.value = value
    self.next = next
  }
}

extension Node: CustomStringConvertible {
  public var description: String {
    guard let next = next else {
      return "\(value)"
    }
    return "\(value) -> \(next)"
  }
}

struct LinkedList<Value> {
  var head: Node<Value>?
  var tail: Node<Value>?
  
  init() {}
  
  var isEmpty: Bool {
    return head == nil
  }
}

extension LinkedList: CustomStringConvertible {
  public var description: String {
    guard let head = head else {
      return ""
    }
    return "\(head)"
  }
}

extension LinkedList {
  mutating func push(_ value: Value) {
    head = Node(value, next: head)
    if tail == nil {
      tail = head
    }
  }
  
  mutating func append(_ value: Value) {
    guard !isEmpty else {
      push(value)
      return
    }
    tail?.next = Node(value)
    tail = tail?.next
  }
  
  mutating func insert(_ value: Value, after node: Node<Value>?) {
    guard tail !== node else {
      append(value)
      return
    }
    
    node?.next = Node(value, next: node?.next)
    
  }
  
  func node(at index: Int) -> Node<Value>? {
    var currentNode = head
    var currentIndex = 0
    
    while currentNode != nil && currentIndex < index {
      currentNode = currentNode?.next
      currentIndex += 1
    }
    
    return currentNode
  }
}

extension LinkedList {
  @discardableResult
  mutating func pop() -> Node<Value>? {
    defer {
      head = head?.next
      if isEmpty {
        tail = nil
      }
    }
    return head
  }
  
  @discardableResult
  mutating func removeLast() -> Node<Value>? {
    guard let head = head else { return nil }
    guard head.next != nil else { return pop() }
    
    var current = head
    var previous = head
    
    while let next = current.next {
      previous = current
      current = next
    }
    
    previous.next = nil
    tail = previous
    return current
  }
  
  @discardableResult
  mutating func remove(after node: Node<Value>?) -> Node<Value>? {
    defer {
      if node?.next === tail {
        tail = node
      }
      node?.next = node?.next?.next
    }
    return node?.next
  }
}

extension LinkedList {
  struct Index: Comparable {
    var node: Node<Value>?
    
    static func <(lhs: Index, rhs: Index) -> Bool {
      guard lhs != rhs else { return false }
      
      let seq = sequence(first: lhs.node?.next) { $0?.next }
      return seq.contains { $0 === rhs.node }
    }
    
    static func ==(lhs: Index, rhs: Index) -> Bool {
      switch (lhs.node, rhs.node) {
      case let (left?, right?):
        return left === right
      case (nil, nil):
        return true
      default:
        return false
      }
    }
  }
}

extension LinkedList: Collection {
  var startIndex: Index {
    return Index(node: head)
  }
  
  var endIndex: Index {
    return Index(node: tail?.next)
  }
  
  func index(after i: Index) -> Index {
    return Index(node: i.node?.next)
  }
  
  subscript(_ index: Index) -> Value {
    return index.node!.value
  }
}

# Print Linked List in reverse

extension LinkedList {
  func printReverseR(_ node: Node<Value>?) {
    guard node != nil else { return }
    printReverseR(node?.next)
    print(node!.value)
  }
  
  func printReverseI() {
    var stack = Stack<Value>()
    
    var current = head
    while let next = current {
      stack.push(next.value)
      current = next.next
    }
    
    while let node = stack.pop() {
      print(node)
    }
  }
  
  func printReverseS() -> String {
    var result = ""
    var current = head
    while let next = current {
      current = current?.next
      result = "\(next.value)" + result
    }
    return result
  }
}

struct Stack<Element> {
  private var elements: [Element] = []
  
  mutating func pop() -> Element? {
    return elements.popLast()
  }
  
  mutating func push(_ element: Element) {
    return elements.append(element)
  }
  
}

# middle node of linked list

//"Create a function that returns the middle node of a linked list",

import Darwin

extension LinkedList {
  func middle() -> Node<Value> {
    var current = head
    var counter = 0
    
    while current != nil {
      current = current?.next
      counter += 1
    }
    
    let middleIndex = counter / 2
    current = head
    counter = 0
    
    while current != nil && counter < middleIndex {
      current = current?.next
      counter += 1
    }
    
    return current!
  }
}


//"Create a function that returns the middle node of a linked list",

import Darwin

extension LinkedList {
  func middle() -> Node<Value> {
    var slowPointer = head
    var fastPointer = head
    
    while fastPointer?.next != nil {
      slowPointer = slowPointer?.next
      fastPointer = fastPointer?.next?.next
    }
    
    return slowPointer!
  }
}


//"Create a function that returns the middle node of a linked list",

import Darwin

extension LinkedList {
  func middle() -> Node<Value> {
    var counter = 0
    var current = head
    var mid = head
    
    while current != nil {
      if counter % 2 != 0 {
        mid = mid?.next
      }
      
      current = current?.next
      counter += 1
    }
    
    return mid!
  }
}

struct Stack<Element> {
  private var elements: [Element] = []
  
  mutating func pop() -> Element? {
    return elements.popLast()
  }
  
  mutating func push(_ element: Element) {
    return elements.append(element)
  }
  
}

var ll = LinkedList<Int>()
ll.append(1)
ll.append(2)
ll.append(3)
ll.append(4)
ll.append(3)
ll.append(4)
ll.middle().value




extension LinkedList {
  func reverseI() -> Node<Value>? {
    var current = head
    var previous: Node<Value>?
    var next: Node<Value>?
    
    while current != nil {
      next = current?.next
      current?.next = previous
      previous = current
      current = next
    }
    return previous
  }
  
  func reverseR(_ node: Node<Value>?) -> Node<Value>? {
    guard let node = node else { return nil }
    guard node.next != nil else { return node }
    let head = reverseR(node.next)
    node.next?.next = node
    node.next = nil
    return head
  }
}



//Binary Search - Recursive
// O(log n)
// O(log n)

extension RandomAccessCollection where Element: Comparable {
  func binarySearch(for target: Element, in range: Range<Index>? = nil) -> Bool {
    let range = range ?? startIndex..<endIndex
    guard range.lowerBound < range.upperBound else { return false }
    
    let size = distance(from: range.lowerBound, to: range.upperBound)
    let middleIndex = index(range.lowerBound, offsetBy: size / 2)
    let middleValue = self[middleIndex]
    
    if middleValue == target {
      return true
    }
    
    if middleValue > target {
      return binarySearch(for: target, in: range.lowerBound..<middleIndex)
    }
    
    if middleValue < target {
      return binarySearch(for: target, in: index(after: middleIndex)..<range.upperBound)
    }
    return false
  }
}

var a = [1, 2, 3, 4, 5, 6, 7, 8, 9]
a.binarySearch(for: 10)



// Jewels and Stones - custom operators

extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return reduce(0) { predicate($1) ? $0 + 1 : $0 }
  }
}

class Solution {
  func numJewelsInStones(_ J: String, _ S: String) -> Int {
    return S.count(where: J.contains)
  }
}



// Jewels and Stones - custom operators

infix operator ~

extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return reduce(0) { predicate($1) ? $0 + 1 : $0 }
  }
}

extension Sequence where Element: Hashable {
  func occurences(in other: Self) -> Int {
    let elementSet = Set(self)
    return other.count(where: elementSet.contains)
  }
  
  static func ~(lhs: Self, rhs: Self) -> Int {
    return lhs.occurences(in: rhs)
  }
}

class Solution {
  func numJewelsInStones(_ J: String, _ S: String) -> Int {
    return J ~ S
  }
}

Solution().numJewelsInStones("aA", "aAAbbbb")






extension Sequence where Element: Hashable {
  var frequencies: [Element: Int] {
    let frequencyPairs = self.map { ($0, 1) }
    return Dictionary(frequencyPairs, uniquingKeysWith: +)
  }
}

let frequencies = "hello".frequencies // ["e": 1, "o": 1, "l": 2, "h": 1] frequencies. lter { $0.value > 1 } // ["l": 2]






extension Sequence where Element: Hashable {
  func unique() -> [Element] {
    var set = Set<Element>()
    return filter { element in
      if set.contains(element) {
        return false
      } else {
        set.insert(element)
        return true
      }
    }
  }
}

[1, 1, 2, 3, 3, 4, 5, 6, 6, 6, 7, 8, 8].unique()





//LinkedList


//"Create a function that prints out the elements of a linked list in reverse order",


//"Create a function that returns the middle node of a linked list",


//"Create a function that reverses a linked list",


//"Create a function that takes two sorted linked lists and merges them into a single sorted linked list",


//"Create a function that removes all occurrences of a specific element from a linked list",


//"Delete node in a linked list",


//"Remove linked list elements",

class Solution {
  func removeElements(_ head: ListNode?, _ val: Int) -> ListNode? {
    let fakeHead = ListNode(-1)
    fakeHead.next = head
    var previous = fakeHead
    
    while let current = previous.next {
      if current.val == val {
        previous.next = current.next
      } else {
        previous = current
      }
    }
    return fakeHead.next
  }
}

class Solution:
    def removeElements(self, head, val):
        """
        :type head: ListNode
        :type val: int
        :rtype: ListNode
        """
        if not head:
            return []

        o_head = ListNode(None)
        o_head.next = head
        head = o_head

        while o_head and o_head.next:
            if o_head.next.val == val:
                o_head.next = o_head.next.next
            else:
                o_head = o_head.next

        return head.next

//"Intersection of two linked lists",


//"Linked list cycle",


//"Palindrome linked list"





## Challenge 1: Are the letters unique?

extension Sequence where Element: Hashable {
    func unique() -> [Element] {
        var uniqueElements = Set<Element>()
        return filter {
            if uniqueElements.contains($0) {
                return false
            } else {
                uniqueElements.insert($0)
                return true
            }
        }
    }
}

extension Collection where Element: Hashable {
    func unique() -> Bool {
        return self.count == Set(self).count
    }
}

func challenge1(input: String) -> Bool {
    return input.count == input.unique().count
}


func challenge1(input: String) -> Bool {
  return input.count == Set(input).count
}

assert(challenge1(input: "No duplicates") == true, "Challenge 1 failed")
assert(challenge1(input: "abcdefghijklmnopqrstuvwxyz") == true, "Challenge 1 failed")
assert(challenge1(input: "AaBbCc") == true, "Challenge 1 failed")
assert(challenge1(input: "Hello, world") == false, "Challenge 1 failed")

















## Challenge 2: Is a string a palindrome?

extension String {
    var isPalindrome: Bool {
        var left = startIndex
        var right = index(before: endIndex)
        
        while left <= right {
            let leftLetter = lowercasedLetter(from: self[left])
            let rightLetter = lowercasedLetter(from: self[right])
            
            if leftLetter != rightLetter {
                return false
            }
            left = index(after: left)
            right = index(before: right)
        }
        return true
    }
    
    func lowercasedLetter(from character: Character) -> String {
        return String(character).lowercased()
    }
}

"rotator".isPalindrome
"Rats live on no evil star".isPalindrome
"Never odd or even".isPalindrome
"Hello, world".isPalindrome

extension String {
  func palindrome() -> Bool {
    let lowercased = self.lowercased()
    return lowercased == String(lowercased.reversed())
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
}

extension String {
    var isPalindrome: Bool {
        let lowercasedString = self.lowercased()
        return lowercasedString == String(lowercasedString.reversed())
    }
    
    var isPalindrome2: Bool {
        let lowercasedString = self.lowercased()
        var stack = Stack<Element>()
        
        for element in lowercasedString {
            stack.push(element)
        }
        
        var reversedString = ""
        while let element = stack.pop() {
            reversedString.append(element)
        }
        return lowercasedString == reversedString.lowercased()
    }
}

"Hello, world".isPalindrome


extension String {
  subscript(position: Int) -> Character {
    return self[index(startIndex, offsetBy: position)]
  }
}

"Hello"[1]

extension String {

  func palindrome1() -> Bool {
    let lowercased = self.lowercased()
    return lowercased == String(lowercased.reversed())
  }

  func palindrome2() -> Bool {
    let lowercased = self.lowercased()
    var reversedString = ""
    for character in lowercased {
      reversedString = "\(character)" + reversedString
    }
    return reversedString == lowercased
  }

  func palindrome3() -> Bool {
    let lowercased = self.lowercased()
    var reversedString = ""

    var stack = Stack<Character>()
    for character in self {
      stack.push(character)
    }

    while let char = stack.pop() {
      reversedString += "\(char)"
    }
    return reversedString == lowercased
  }

  func palindrome4() -> Bool {
    let lengthHalf = count / 2
    var lengthOne = count - 1

    var i = 0
    while i < lengthHalf {
      if self[i] != self[lengthOne] {
        return false
      }
      i += 1
      lengthOne -= 1
    }
    return true
  }


extension String {
  func pal1() -> Bool {
    var left = startIndex
    var right = index(before: endIndex)
    
    while left <= right {
      if self[left] != self[right] {
        return false
      }
      left = index(after: left)
      right = index(before: right)
    }
    return true
  }
}

"racecar".pal1()

  
}

extension String {
  func palindrome4() -> Bool {
    let size = distance(from: startIndex, to: endIndex)
    let middleIndex = index(startIndex, offsetBy: size / 2)
    
    var leftIndex = startIndex
    var rightIndex = index(before: endIndex)
    
    while leftIndex < middleIndex {
      if self[leftIndex] != self[rightIndex] {
        return false
      }
      leftIndex = index(after: leftIndex)
      rightIndex = index(before: rightIndex)
    }
    return true
  }
}

struct Stack<Value> {
  var elements = [Value]()

  mutating func push(_ value: Value) {
    elements.append(value)
  }

  mutating func pop() -> Value? {
    return elements.popLast()
  }
}

"racecar".palindrome4()

struct Stack<Element> {
    var elements: [Element] = []
    
    mutating func push(_ element: Element) {
        elements.append(element)
    }
    
    mutating func pop() -> Element? {
        return elements.popLast()
    }
}

extension String {
    subscript(index: Int) -> String {
        return String(self[self.index(startIndex, offsetBy: index)])
    }
}

extension String {
    var isPalindrome: Bool {
        return self == String(self.reversed())
    }
    
    var isPalindrome2: Bool {
        var stack = Stack<String>()
        
        for element in self {
            stack.push("\(element)")
        }
        
        var reversedString = ""
        while let character = stack.pop() {
            reversedString.append(character)
        }
        
        return self == reversedString
    }
    
    var isPalindrome3: Bool {
        var left = startIndex
        var right = index(before: endIndex)
        
        while left <= right {
            if self[left] != self[right] {
                return false
            }
            left = index(after: left)
            right = index(before: right)
        }
        return true
    }
}



## Challenge 3: Do two strings contain the same characters?

extension Sequence where Element: Hashable {
    func sameElements(as other: Self) -> Bool {
        return frequencies() == other.frequencies()
    }

    func frequencies() -> [Element: Int] {
        return reduce(into: [:]) { counts, element in
            counts[element, default: 0] += 1
        }
    }
}

func sameCharacters(_ string1: String, _ string2: String) -> Bool {
  let array1 = Array(string1)
  let array2 = Array(string2)
  return array1.sorted() == array2.sorted()
}

extension String {
    func sameCharacters(as string: String) -> Bool {
        let sorted1 = self.sorted()
        let sorted2 = string.sorted()
        
        return sorted1 == sorted2
    }
    
    func sameCharacters2(as string: String) -> Bool {
        var elementsDictionary: [Element: Int] = [:]
        for element in self {
            elementsDictionary[element] = (elementsDictionary[element] ?? 0) + 1
        }
        
        for element in string {
            if let count = elementsDictionary[element], count > 0 {
                elementsDictionary[element] = count - 1
            } else {
                return false
            }
        }
        return true
    }
}

"abca".sameCharacters(as: "abca")

//For bonus interview points, you might be tempted to write something like this:
//
//return array1.count == array2.count && array1.sorted() ==
//  array2.sorted()
//
//However, there’s no need – the == operator for arrays does that before doing its item-by-item comparison, so doing it yourself is just redundant.
// conformance to the equatable protocol makes this check in a guard on the static == func


















## Challenge 4: Does one string contain another?

import Foundation

extension String {
   func fuzzyContains(_ string: String) -> Bool {
      return range(of: string, options: .caseInsensitive) != nil
    }
}

"Hello, world".fuzzyContains("WORLD")




// Challenge 5: Count the characters

import Foundation

extension String {
  func count4(for character: String) -> Int {
    let modified = replacingOccurrences(of: character, with: "")
    return count - modified.count
  }
}

import Foundation

extension Sequence where Element: Hashable {
    func count(of element: Element) -> Int {
        return reduce(0) {
            $1 == element ? $0 + 1 : $0
        }
    }
    
    func count2(of element: Element) -> Int {
        let countedSet = NSCountedSet(array: Array(self))
        return countedSet.count(for: element)
    }
    
    func count3(of element: Element) -> Int {
        var counts: [Element: Int] = [:]
        for element in self {
            counts[element] = counts[element] ?? 0 + 1
        }
        return counts[element] ?? 0
    }
    
    func count(where predicate: (Element) -> Bool) -> Int {
        var count = 0
        
        for element in self where predicate(element) {
            count += 1
        }
        return count
    }
}

extension String {
    func count4(of element: Element) -> Int {
        let modifiedCount = self.replacingOccurrences(of: "\(element)", with: "").count
        return self.count - modifiedCount
    }
}

"Hello".count4(for: "l")

func challenge5d(input: String, count: String) -> Int {
   let modified = input.replacingOccurrences(of: count, with:
"")
   return input.count - modified.count
}

import Foundation

extension Sequence where Element: Hashable {
    func count(for element: Element) -> Int {
        return reduce(0) {
            $1 == element ? $0 + 1 : $0
        }
    }
    
    func count2(for element: Element) -> Int {
        let countedSet = NSCountedSet(array: Array(self))
        return countedSet.count(for: element)
    }
    
    func count3(for element: Element) -> Int {
        var elementCount = [Element: Int]()
        
        for element in self {
            elementCount[element] = (elementCount[element] ?? 0) + 1
        }
        
        return elementCount[element] ?? 0
    }
}

extension Collection where Element: Equatable {
    func count4(for element: Element) -> Int {
        return count - filter { $0 != element }.count
    }
}

extension String {
    func count5(for element: String) -> Int {
        let string = replacingOccurrences(of: element, with: "")
        return count - string.count
    }
}


## Challenge 6 - Remove duplicate letters from a string

import Foundation

extension Sequence where Element: Hashable {
    func removeDuplicates() -> [Element] {
        var elements = Set<Element>()
        return filter {
            if elements.contains($0) {
                return false
            } else {
                elements.insert($0)
                return true
            }
        }
    }
}

extension String {
    func removeDuplicates() -> String {
        var elements = Set<Element>()
        return filter {
            if elements.contains($0) {
                return false
            } else {
                elements.insert($0)
                return true
            }
        }
    }
    
    func removeDuplicates2() -> String {
        let array = self.map { String($0) }
        let set = NSOrderedSet(array: array)
        let letters = Array(set) as! Array<String>
        return letters.joined()
    }
    
    func removeDuplicates3() -> String {
        var used: [Character: Bool] = [:]
        
        return filter {
            used.updateValue(true, forKey: $0) == nil
        }
    }
}

let string = "hello".removeDuplicates2()


var a: [Int: Int] = [:]
a.updateValue(5, forKey: 1)
a.updateValue(4, forKey: 1)
a.updateValue(3, forKey: 1)



extension String {
    func unique() -> String {
        var elements: [Character: Bool] = [:]
        
        return filter {
            elements.updateValue(true, forKey: $0) == nil
        }
    }
}




## Challenge 7 - Remove duplicate letters from a string

extension String {
    func condenseWhitespace() -> String {
        var seenSpace = false
        return filter {
            if $0 == " " {
                if seenSpace {
                    return false
                }
                seenSpace = true
                return true
            }
            seenSpace = false
            return true
        }
    }
}


import Foundation

extension String {
    func condense() -> String {
        return replacingOccurrences(of: " +", with: " ", options: .regularExpression)
    }
    
    func condense2() -> String {
        var result = ""
        var seenSpace = false
        
        for character in self {
            if seenSpace && character == " " {
                continue
            }
            
            seenSpace = character == " "
            result.append(character)
        }
        return result
    }
    
    func condense3() -> String {
        var seenSpace = false
        return filter {
            if seenSpace && $0 == " " {
                return false
            }

            seenSpace = $0 == " "
            return true
        }
    }
}

"abc ".condense3()




import Foundation

extension String {
    func condenseWhitespace() -> String {
        return replacingOccurrences(of: " +", with: " ", options: .regularExpression, range: nil)
    }
}

"       a c".condenseWhitespace()

import Foundation

extension String {
    func condenseWhitespace() -> String {
        return replacingOccurrences(of: " +", with: " ", options: .regularExpression)
    }
    
    func condenseWhitespace2() -> String {
        var spaceSeen = false
        return filter {
            if $0 == " " {
                if spaceSeen {
                    return false
                }
                spaceSeen = true
                return true
            } else {
                spaceSeen = false
                return true
            }
        }
    }
}


" a        cc     s   aaa    q".condenseWhitespace2()



## Challenge 8: String is rotated

https://www.youtube.com/watch?v=ZMQWjslBlbU

import Foundation

extension String {
    func rotation(of string: String) -> Bool {
        guard count == string.count else {
            return false
        }
        let rotations = self + self
        return rotations.contains(string)
    }
}


"abcde".rotation(of: "abced")


# Challenge 9: Find pangrams

import Foundation

extension String {
    var isPangram: Bool {
        let set = Set(lowercased())
        let letters = set.filter { $0 >= "a" && $0 <= "z" }
        return letters.count == 26
    }
}

# Challenge 10: Vowels and consonants

extension String {
    var vowelsAndConsonants: (vowels: Int, consonants: Int) {

        let vowels = Set(["A", "E", "I", "O", "U"])
        let consonants = Set(["B", "C", "D", "F", "G", "H", "J", "K", "L", "M", "N", "P",
                              "Q", "R", "S", "T", "V", "W", "X", "Y", "Z"])

        var vowelCount = 0
        var consonantCount = 0

        for element in self {
            let letter = "\(element)".uppercased()
            if vowels.contains(letter) {
                vowelCount += 1
                continue
            }

            if consonants.contains(letter) {
                consonantCount += 1
                continue
            }
        }
        return (vowels: vowelCount, consonants: consonantCount)
    }
}

extension String {
    var vowelsAndConsonants: (vowels: Int, consonants: Int) {

        let vowels = Set<Character>(["A", "E", "I", "O", "U"])
        let consonants = Set<Character>(["B", "C", "D", "F", "G", "H", "J", "K", "L", "M", "N", "P",
                              "Q", "R", "S", "T", "V", "W", "X", "Y", "Z"])

        var vowelCount = 0
        var consonantCount = 0

        for letter in self.uppercased() {
            if vowels.contains(letter) {
                vowelCount += 1
                continue
            }

            if consonants.contains(letter) {
                consonantCount += 1
                continue
            }
        }
        return (vowels: vowelCount, consonants: consonantCount)
    }
}

var a = "Swift Coding Challenges"
a.vowelsAndConsonants







# Challenge 11: Three different letters

extension String {
    func threeDiff(to other: String) -> Bool {
        guard count == other.count else {
            return false
        }
        
        var differences = 0
        
        for (i, j) in zip(self, other) {
            guard differences < 3 else {
                return false
            }
            
            if i != j {
                differences += 1
            }
        }
        return true
    }
}

"clamp".threeDiff(to: "maple")


extension String {
    func check(against other: String) -> Bool {
        guard self.count == other.count else {
            return false
        }
        
        let string1 = Array(self)
        let string2 = Array(other)
        
        var differenceCount = 0
        
        for (index, value) in string1.enumerated() {
            guard differenceCount < 3 else {
                return false
            }
            
            if value != string2[index] {
                differenceCount += 1
            }
        }
        return true
    }
}






# Challenge 12: Find longest prefix

import Foundation

extension String {
    func longestPrefix() -> String {
        guard count > 0 else { return "" }
        
        let strings = self.components(separatedBy: " ")
        guard let first = strings.first else { return "" }
        
        var currentPrefix = ""
        var bestPrefix = ""
        
        for letter in first {
            currentPrefix.append(letter)
            
            for word in strings {
                if !word.hasPrefix(currentPrefix) {
                    return bestPrefix
                }
            }
            
            bestPrefix = currentPrefix
        }
        return bestPrefix
    }
}


## Challenge 13

extension String {
    func runLength() -> String {
        guard let first = self.first else { return self }
        
        var result = ""
        var currentCharacter = "\(first)"
        var currentCount = 1
        
        for character in self.dropFirst() {
            guard currentCharacter != "\(character)" else {
                currentCount += 1
                continue
            }
            
            defer {
                currentCharacter = "\(character)"
                currentCount = 1
            }
            result.append("\(currentCharacter)\(currentCount)")
        }
        
        result.append("\(currentCharacter)\(currentCount)")
        return result
    }
}

"aaAAaa".runLength()

extension String {
    func encode() -> String {
        guard let first = first else { return self }
        
        var result = ""
        var currentCount = 1
        var currentCharacter = first
        
        for character in dropFirst() {
            if character == currentCharacter {
                currentCount += 1
            } else {
                result += "\(currentCharacter)\(currentCount)"
                currentCount = 1
                currentCharacter = character
            }
        }
        
        result += "\(currentCharacter)\(currentCount)"
        return result
    }
}

"aaabaaabaaa".encode()


## Challenge 14

extension String {
    func permutations(_ current: String = "") {
        let strArray = Array(self)
        
        guard !isEmpty else {
            print(current)
            return
        }
        
        for i in strArray.indices {
            let left = String(strArray[0..<i])
            let right = String(strArray[i + 1..<count])
            let string = left + right
            string.permutations(current + String(strArray[i]))
        }
    }
}

"abc".permutations()



# Challenge 15

import Foundation

extension String {
    func reversedWords() -> String {
        return components(separatedBy: " ")
                .map { String($0.reversed()) }
                .joined(separator: " ")
    }
}

"Swift Coding Challenges".reversedWords()



infix operator |>
func |> <A, B>(a: A, f: (A) -> B) -> B {
    return f(a)
}

import Foundation

extension String {
    func reverseWords() -> String {
        guard !isEmpty else { return self }
        return components(separatedBy: " ")
            .map { $0.reversed() |> String.init }
            .joined(separator: " ")
    }
}

"Swift Coding Challenges".reverseWords()





# Challenge 16

struct UnfoldingSequence<State, T>: Sequence, IteratorProtocol {
    let _next: (inout State) -> T?
    var state: State
    
    init(state: State, next: @escaping (inout State) -> T?) {
        self.state = state
        self._next = next
    }
    
    mutating func next() -> T? {
        return _next(&state)
    }
}

func value(for number: Int) -> String {
    if number.divisible(by: 15) { return "FizzBuzz" }
    if number.divisible(by: 3)  { return "Fizz" }
    if number.divisible(by: 5)  { return "Buzz" }
    return "\(number)"
}

extension Int {
    func divisible(by divisor: Int) -> Bool {
        return self % divisor == 0
    }
}

let fizzBuzz = UnfoldingSequence(state: 1) { state -> String in
    defer { state += 1 }
    return value(for: state)
}

for value in fizzBuzz.prefix(16) {
    print(value)
}

# Challenge 17
import Darwin

extension Int {
    static func random(in range: Range<Int>) -> Int {
        return range.lowerBound + Int(arc4random_uniform(UInt32(range.upperBound - range.lowerBound)))
    }
    
    static func random(in range: ClosedRange<Int>) -> Int {
        return range.lowerBound + Int(arc4random_uniform(UInt32(range.upperBound - range.lowerBound) + 1))
    }
}

Int.random(in: 1...5)
Int.random(in: 1...5)
Int.random(in: 1...5)
Int.random(in: 1...5)
Int.random(in: 1...5)
Int.random(in: 1...5)
Int.random(in: 1...5)
Int.random(in: 1...5)
Int.random(in: 1...5)
Int.random(in: 1...5)



var a = 1..<5
a.upperBound
a.lowerBound


## Challenge 18

```swift
extension Int {
    func pow(to power: Int) -> Int {
        guard self > 0 && power > 0 else {
            return 0
        }
        
        var result = 1
        
        for _ in 0..<power {
            result *= self
        }
        
        return result
    }
    
    func powR(to power: Int) -> Int {
        guard power > 0 else {
            return 1
        }
        
        var result = self
        result *= powR(to: power - 1)
        return result
    }
}

2.powR(to: 8)

```



## Challenge 19

```swift
func swap(a: inout Int, b: inout Int) {
    swap(&a, &b)
}

func swap1(a: inout Int, b: inout Int) {
    a = a ^ b
    b = a ^ b
    a = a ^ b
}

func swap2(a: inout Int, b: inout Int) {
    (a, b) = (b, a)
}

var a = 1
var b = 2

swap2(a: &a, b: &b)

a
b
```


# Challenge 20

```swift
extension Int {
    var isPrime: Bool {
        guard self >= 2 else { return false }
        guard self != 2 else { return true }
        let max = Int(ceil(sqrt(Double(self))))
        for i in 2...max {
          if self % i == 0 {
             return false
          }
        }
        return true
    }
}

16_777_259.isPrime
```


## Challenge 37

```swift
extension Collection where Element == Int {
    func count(of element: Character) -> Int {
        return reduce(0) {
            $0 + String($1).filter { $0 == element }.count
        }
    }
}

[5, 15, 55, 515].count(of: "5")

```

## Challenge 38

```swift
extension Collection where Element: Comparable {
    func minElements(through count: Int) -> [Element] {
        let sorted = self.sorted()
        return Array(sorted.prefix(count))
    }
}

[1, 5, 4, 6, 5, 7, 6, 3, 2, 8, 7, 8, 9, 8, 7].minElements(through: 5)

```


## Challenge 39

```swift
extension Collection where Element == String {
    func lengthSort(by areInIncreasingOrder: (Element, Element) throws -> Bool) rethrows -> [Element] {
        return try self.sorted(by: areInIncreasingOrder)
    }
}

["a", "abc", "ab"].lengthSort(by: >)

```

## Challenge 40

```swift
extension Collection where Element == Int {
    func missingNumbers() -> [Element] {
        let numbers = Set(self)
        let comparisionNumbers = Set(1...100)
        return Array(numbers.symmetricDifference(comparisionNumbers))
    }
}

var testArray = Array(1...100)
testArray.remove(at: 25)
testArray.remove(at: 20)
testArray.remove(at: 6)
testArray.missingNumbers()

```


## Challenge 41

```swift
extension Collection where Element == Int, Index == Int {
    func median() -> Double? {
        guard !isEmpty else { return nil }
    
        let sorted = self.sorted()
        let middle = sorted.count / 2
    
        if isEvenCount {
            return Double(sorted[middle] + sorted[middle - 1]) / 2
        } else {
            return Double(self[middle])
        }
    }
}

extension Collection {
    var isEvenCount: Bool {
        return count % 2 == 0
    }
}

[1, 3, 5, 7, 9].median()
[1, 3, 5].median()
[1, 2, 3, 4].median()


```



## Challenge 42

```swift
extension Collection where Element: Equatable {
    func index(of element: Element) -> Index? {
        for index in indices {
            if self[index] == element {
                return index
            }
        }
        return nil
    }
}

[1, 2, 3].index(of: 5)

```


## Challenge 43

```swift
class Node<Value> {
    let value: Value
    var next: Node<Value>?
    
    init(_ value: Value, next: Node<Value>? = nil) {
        self.value = value
        self.next = next
    }
}

extension Node: CustomStringConvertible {
    var description: String {
        guard let next = next else {
            return "\(value)"
        }
        return "\(value) \(next)"
    }
}

struct LinkedList<Value> {
    var head: Node<Value>?
    var tail: Node<Value>?
    
    var isEmpty: Bool {
        return head == nil
    }
}

extension LinkedList {
    mutating func push(_ value: Value) {
        head = Node(value, next: head)
        if tail == nil {
            tail = head
        }
    }
    
    mutating func append(_ value: Value) {
        guard !isEmpty else {
            push(value)
            return
        }
        tail?.next = Node(value)
        tail = tail?.next
    }
}

extension LinkedList: CustomStringConvertible {
    var description: String {
        guard let head = head else {
            return ""
        }
        return "\(head)"
    }
}

var linkedList = LinkedList<String>()
for letter in "abcdefghijklmnopqrstuvwxyz" {
    linkedList.append(String(letter))
}
print(linkedList)

```



## Challenge 44

```swift
class Node<Value> {
    let value: Value
    var next: Node<Value>?
    
    init(_ value: Value, next: Node<Value>? = nil) {
        self.value = value
        self.next = next
    }
}

var node1 = Node(1)
var node2 = Node(2)
var node3 = Node(3)
var node4 = Node(4)
var node5 = Node(5)
node1.next = node2
node2.next = node3
node3.next = node4
node4.next = node5

func midPoint<Value>(_ node: Node<Value>) -> Node<Value>? {
    var slowPointer: Node<Value>? = node
    var fastPointer: Node<Value>? = node
    
    while fastPointer?.next != nil && fastPointer != nil {
        slowPointer = slowPointer?.next
        fastPointer = fastPointer?.next?.next
    }
    return slowPointer
}

midPoint(node1)!.value

```














struct UnfoldingSequence<State, T>: Sequence, IteratorProtocol {
  let _next: (inout State) -> T?
  var state: State
  
  init(state: State, next: @escaping (inout State) -> T?) {
    _next = next
    self.state = state
  }
  
  mutating func next() -> T? {
    return _next(&state)
  }
}

let fibs = UnfoldingSequence(state: (0, 1)) { state -> Int? in
  defer { state = (state.1, state.0 + state.1) }
  return state.0
}

for f in fibs.prefix(10) {
  print(f)
}


extension String {
  func unique() -> String {
    var seenElements = Set<Character>()
    return filter { element in
      if seenElements.contains(element) {
        return false
      } else {
        seenElements.insert(element)
        return true
      }
    }
  }
}

"Mississippi".unique()



extension String {
  func unique() -> String {
    var seenElements = Set<Character>()
    return reduce("") { result, element in
      if seenElements.contains(element) {
        return result
      } else {
        seenElements.insert(element)
        return result + "\(element)"
      }
    }
  }
}

"Hello".unique()



//"Create a function that prints out the elements of a linked list in reverse order",

struct Stack<Value> {
  var elements: [Value] = []
  
  mutating func push(_ value: Value) {
    elements.append(value)
  }
  
  mutating func pop() -> Value? {
    return elements.popLast()
  }
}

//func r<Value>(_ node: Node<Value>?) {
//  var stack = Stack<Value>()
//  var current = node
//
//  while current != nil {
//    stack.push(current!.value)
//    current = current!.next
//  }
//
//  while let value = stack.pop() {
//    print(value)
//  }
//}

//func r<Value>(_ node: Node<Value>?) -> String {
//  var current = node
//  var result = ""
//  while current != nil {
//    result = "\(current!.value)" + " " + result
//    current = current!.next
//  }
//  return result
//}

func r<Value>(_ node: Node<Value>?) {
  guard node != nil else { return }
  r(node?.next)
  print(node!.value)
}

r(node4)


//"Create a function that returns the middle node of a linked list",

func middle<Value>(_ node: Node<Value>?) -> Node<Value>? {
  var current = node
  var previous = node
  
  while current != nil {
    current = current?.next?.next
    previous = previous?.next
  }
  return previous?.next
}

middle(node4)



func middle<Value>(_ node: Node<Value>?) -> Node<Value>? {
  var current = node
  var previous = node
  var count = 0
  
  while current != nil {
    count += 1
    
    if count % 2 != 0 {
      previous = previous?.next
    }
    
    current = current?.next
    
  }
  return previous
}

middle(node4)









//"Remove linked list elements",

class Solution {
  func removeElements(_ head: ListNode?, _ val: Int) -> ListNode? {
    var current = head
    var previous: ListNode?
    
    while current != nil {
      if current?.val == val {
        previous?.next = current?.next
      }
      previous = current
      current = current?.next
    }
    return head
  }
  
  func removeElementsR(_ head: ListNode?, _ val: Int) -> ListNode? {
    if head == nil {
      return nil
    }
    
    head?.next = removeElementsR(head, val)
    
    return head?.val == val ? head?.next : head
  }
}

Solution().removeElements(node1, 6)


//"Delete node in a linked list",

func delete(_ head: ListNode?, _ val: Int) {
  var current = head
  var previous = head
  
  while current != nil {
    if current?.val == val {
      previous?.next = current?.next
      return
    }
    previous = current
    current = current?.next
  }
}

print(node1)
delete(node1, 3)
print(node1)



//"Linked list cycle",

func cycle(_ node: ListNode?) -> Bool {
  var slow = node
  var fast = node
  
  while slow !== fast {
    if fast == nil || fast?.next == nil {
      return false
    }
    fast = fast?.next?.next
    slow = slow?.next
  }
  return true
}

cycle(node1)



challenge 6
extension String {
  func unique1() -> String {
    var seenElements = Set<Character>()
    return filter { character in
      if seenElements.contains(character) {
        return false
      } else {
        seenElements.insert(character)
        return true
      }
    }
  }
  
  func unique2() -> String {
    var seenElements = Set<Character>()
    return reduce("") { result, character in
      if seenElements.contains(character) {
        return result
      } else {
        seenElements.insert(character)
        return result + "\(character)"
      }
    }
  }

    func unique2() -> String {
    var used = [Character: Bool]()
    return filter { used.updateValue(true, forKey: $0) == nil }
  }
}



extension String {
  func removeDuplicates() -> String {
    var seenCharacters = [Character: Bool]()
    
    let result = filter {
      seenCharacters.updateValue(true, forKey: $0) == nil
    }
    
    return String(result)
  }
}


// merge linked lists
class Solution {
  func mergeTwoLists(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
    var l1 = l1
    var l2 = l2
    
      guard l1 != nil else { return l2 }
      guard l2 != nil else { return l1 }
      
    var head: ListNode?

    if l1!.val > l2!.val {
    head = l2
    l2 = l2?.next
    } else {
    head = l1
    l1 = l1?.next
    }

    var current = head

    while l1 != nil && l2 != nil {
    if (l1?.val ?? 0) > (l2?.val ?? 0) {
      current?.next = l2
      l2 = l2?.next
      current = current?.next
    } else {
      current?.next = l1
      l1 = l1?.next
      current = current?.next
    }
    }

    current?.next = l1 != nil ? l1 : l2
    return head
  }
}



class ListNode {
  let val: Int
  var next: ListNode?
  
  init(_ val: Int, next: ListNode? = nil) {
    self.val = val
    self.next = next
  }
}

extension ListNode: CustomStringConvertible {
  var description: String {
    guard let next = next else {
      return "\(val)"
    }
    return "\(val) -> \(next)"
  }
}


let node5 = ListNode(5)
let node4 = ListNode(4, next: node5)
let node3 = ListNode(3, next: node4)
let node2 = ListNode(2, next: node3)
let node1 = ListNode(1, next: node2)

let node9 = ListNode(4)
let node8 = ListNode(2, next: node9)
let node7 = ListNode(2, next: node8)
let node6 = ListNode(1, next: node7)

func merge(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
  var l1 = l1
  var l2 = l2
  let preHead = ListNode(-1)
  var previous: ListNode? = preHead
  
  while l1 != nil && l2 != nil {
    if (l1?.val ?? 0) > (l2?.val ?? 0) {
      previous?.next = l2
      l2 = l2?.next
    } else {
      previous?.next = l1
      l1 = l1?.next
    }
    previous = previous?.next
  }
  
  previous?.next = l1 != nil ? l1 : l2
  return preHead.next
}

merge(node1, node6)





// intersection of two linked lists
class ListNode {
  let val: Int
  var next: ListNode?
  
  init(_ val: Int, next: ListNode? = nil) {
    self.val = val
    self.next = next
  }
}

extension ListNode: CustomStringConvertible {
  var description: String {
    guard let next = next else {
      return "\(val)"
    }
    return "\(val) -> \(next)"
  }
}


let node5 = ListNode(5)
let node4 = ListNode(4, next: node5)
let node3 = ListNode(3, next: node4)
let node2 = ListNode(2, next: node3)
let node1 = ListNode(1, next: node2)

//let node9 = ListNode(4)
//let node8 = ListNode(2, next: )
let node7 = ListNode(2, next: node4)
let node6 = ListNode(1, next: node7)


//Intersection of two linked lists

func inter(_ node1: ListNode?, _ node2: ListNode?) -> ListNode? {
  var l1 = node1
  var l2 = node2
  
  let l1Length = length(l1)
  let l2Length = length(l2)
  
  
  if l1Length < l2Length {
    return inter(l2, l1)
  }
  
  for _ in 0..<l1Length - l2Length {
    l1 = l1?.next
  }

  while l1 != nil && l2 != nil {
    if l1 === l2 {
      return l1
    }

    l1 = l1?.next
    l2 = l2?.next
  }
  return nil
}

func length(_ node: ListNode?) -> Int {
  var current = node
  var count = 0
  
  while current != nil {
    current = current?.next
    count += 1
  }
  
  return count
}

inter(node1, node6)







class ListNode {
  let val: Int
  var next: ListNode?
  
  init(_ val: Int, next: ListNode? = nil) {
    self.val = val
    self.next = next
  }
}

extension ListNode: CustomStringConvertible {
  var description: String {
    guard let next = next else {
      return "\(val)"
    }
    return "\(val) -> \(next)"
  }
}


let node5 = ListNode(5)
let node4 = ListNode(4, next: node5)
let node3 = ListNode(3, next: node4)
let node2 = ListNode(2, next: node3)
let node1 = ListNode(1, next: node2)

//let node9 = ListNode(4)
//let node8 = ListNode(2, next: )
let node7 = ListNode(2, next: node4)
let node6 = ListNode(1, next: node7)


func intersection(_ l1: ListNode?, _ l2: ListNode?) -> ListNode? {
  var l1 = l1
  var l2 = l2
  
  let l1Length = length(l1)
  let l2Length = length(l2)

  if l1Length < l2Length {
    return intersection(l2, l1)
  }

  for _ in 0..<l1Length - l2Length {
    l1 = l1?.next
  }

  while l1 != nil && l2 != nil {
    if l1 === l2 {
      return l1
    }
    l1 = l1?.next
    l2 = l2?.next
  }
  return nil
}

func length(_ node: ListNode?) -> Int {
  var current = node
  var counter = 0
  
  while current != nil {
    counter += 1
    current = current?.next
  }
  return counter
}

intersection(node6, node1)






class Solution {
  func deleteDuplicates(_ head: ListNode?) -> ListNode? {
    let dummy = ListNode(0)
    dummy.next = head
    var previous: ListNode? = dummy
    var current = head
    
    while current != nil {
      while current?.next != nil && previous?.next?.val == current?.next?.val {
        current = current?.next
      }
      
      if previous?.next === current {
        previous = previous?.next
      } else {
        previous?.next = current?.next
      }
      current = current?.next
    }
    
    return dummy.next
  }
}


//Remove Duplicates from Sorted Array - (Leetcode)

class Solution {
  func removeDuplicates(_ nums: inout [Int]) -> Int {
    guard nums.count > 1 else { return nums.count }
    
    var i = 0
    
    for j in 1..<nums.count {
      if nums[i] != nums[j] {
        i += 1
        nums.swapAt(i, j)
      }
    }
    
    return i + 1
  }
}





 //Remove Duplicates from Sorted List

class Solution {
  func deleteDuplicates(_ head: ListNode?) -> ListNode? {
    var current = head
    
    while current != nil && current?.next != nil {
      if current?.val == current?.next?.val {
        current?.next = current?.next?.next
      } else {
       current = current?.next
      }
    }
    return head
  }
}



extension Int {
  static postfix func ~! (number: Int) -> Int {
    return factorial(number)
  }
  
  static func factorial(_ number: Int) -> Int {
    guard number > 1 else { return 1 }
    
    var current = number
    var result = 1
    
    while current > 0 {
      result *= current
      current -= 1
    }
    
    return result
  }
}

3~!







//Challenge 5: Count the characters

import Foundation

extension Sequence where Element: Equatable {
    func count(of element: Element) -> Int {
        return reduce(0) {
            $1 == element ? $0 + 1 : $0
        }
    }
}

"Mississippi".count(of: "i")


extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return reduce(0) {
      predicate($1) ? $0 + 1 : $0
    }
  }
}

extension String {
  func count1(for character: Character) -> Int {
    let set = NSCountedSet(array: Array(self))
    return set.count(for: character)
  }
  
  func count2(for character: Character) -> Int {
    var result = 0
    for element in self where element == character {
      result += 1
    }
    return result
  }
  
  func count3(for character: Character) -> Int {
    return reduce(0) { $1 == character ? $0 + 1 : $0 }
  }
  
  func count4(for character: String) -> Int {
    return count - replacingOccurrences(of: character, with: "").count
  }
  
  func count5(for character: Character) -> Int {
    let occurrences = filter { $0 == character }
    return occurrences.count
  }
  
  func count6(for character: Character) -> Int {
    return count { $0 == character }
  }
}

"Mississippi".count6(for: "i")







extension String {
  func removeExtraSpaces() -> String {
    var seenSpace = false
    return filter { element in
      if element == " " {
        if seenSpace {
          return false
        }
        seenSpace = true
      } else {
        seenSpace = false
      }
      return true
    }
  }
}

"     a   aa a a  a a".removeExtraSpaces()



Challenge 8

extension String {
  func isRotation(of string: String) -> Bool {
    guard count == string.count else { return false }
    return (self + self).contains(string)
  }
}

"ftswi".isRotation(of: "swift")

Time Complexity: Time complexity of this problem depends on the implementation of strstr function.
If implementation of strstr is done using KMP matcher then complexity of the above program is (-)(n1 + n2) where n1 and n2 are lengths of strings. KMP matcher takes (-)(n) time to find a substrng in a string of length n where length of substring is assumed to be smaller than the string.


Challenge 9

extension String {
  func isPanagram() -> Bool {
    let set = Set(lowercased())
    let letters = set.filter { $0 >= "a" && $0 <= "z" }
    return letters.count == 26
  }
}

"The quick brown fox jumps over the lazy dog".isPanagram()


Challenge 10

extension String {
  func vowelsAndConsonants() -> (Int, Int) {
    let vowelsSet: Set<Character> = Set(["a", "e", "i", "o", "u"])
    let string = self.lowercased().replacingOccurrences(of: " ", with: "")
    
    var vowels = 0
    var consonants = 0
    
    for letter in string {
      if vowelsSet.contains(letter) {
        vowels += 1
      } else {
        consonants += 1
      }
    }
    return (vowels, consonants)
  }
}

"Mississippi".vowelsAndConsonants()



//Challenge 10: Vowels and consonants
# if you can garuntee that the input is only letters then you can just fins the voewls and subtract the counts to find the other letters that are not vowels
extension Sequence {
  func count(where predicate: (Element) -> Bool) -> Int {
    return reduce(0) {
      return predicate($1) ? $0 + 1 : $0
    }
  }
}

extension String {
  func vowelsAndConsonants() -> (Int, Int) {
    let vowels = Set<Character>(["a", "e", "i", "o", "u"])
    let vowelCount = lowercased().count(where: vowels.contains)
    let consonantCount = count - vowelCount
    return (vowelCount, consonantCount)
  }
}

"Hello".vowelsAndConsonants()




extension String {
  func vowelsAndConsonants() -> (Int, Int) {
    let vowels = Set<Character>("aeiou")
    let consonants = Set<Character>("bcdfghjklmnpqrstvwxyz")
    
    var vowelCount = 0
    var consonantCount = 0
    
    for character in self.lowercased() {
      if vowels.contains(character) {
        vowelCount += 1
      } else if consonants.contains(character) {
        consonantCount += 1
      }
    }
    
    return (vowelCount, consonantCount)
  }
}

"Hello".vowelsAndConsonants()

// Challenge 11

extension String {
  func threeDifferentLetters(in string: String) -> Bool {
    guard count == string.count else { return false }
    let string1 = Array(self)
    let string2 = Array(string)
    
    var differences = 0
    
    for index in 0..<string1.count {
      if string1[index] != string2[index] {
        differences += 1
        if differences > 3 {
          return false
        }
      }
    }
    return true
  }
}

"clamp".threeDifferentLetters(in: "maple")



//Remove linked list elements

func remove(_ element: Int, in head: ListNode?) -> ListNode? {
  let dummyHead = ListNode(-1)
  dummyHead.next = head
  var current: ListNode? = dummyHead
  
  while current != nil {
    if current?.next?.val == element {
      current?.next = current?.next?.next
    }
    current = current?.next
  }
  return dummyHead.next
}

print(remove(1, in: node1))




//Find of perfect squares between two numbers

extension RandomAccessCollection where Element == Int {
  func squares() -> [Element] {
    var result = [Int]()

    var current = self[startIndex]
    var square = current * current
    let last = self[index(before: endIndex)]

    while square < last {
      result.append(square)
      current += 1
      square = current * current
    }
    return result
  }
}


(1...100).squares()
(1..<100).squares()




//Find of perfect squares between two numbers

import Darwin

extension RandomAccessCollection where Element == Int {
  func perfectSquares() -> [Element] {
    guard count > 1 else { return [] }
    var result = [Int]()
    var start = Int(sqrt(Double(self[startIndex])))
    let end = self[index(before: endIndex)]
    
    while start * start < end {
      result.append(start * start)
      start += 1
    }
    return result
  }
}

(1..<100).perfectSquares()
(1...1000).perfectSquares()
Array(50...100).perfectSquares()

# Infix pow operator

```swift
import Foundation

infix operator ^^ : MultiplicationPrecedence

func ^^(_ radix: Int, power: Int) -> Int {
  return Int(pow(Double(radix), Double(power)))
}

5 ^^ 2

```



# quicksort with new static random method on Int

```swift
public func quickSort<Element: Comparable>(_ array: inout [Element]) {
  return quickSort(&array, array.startIndex, array.index(before: array.endIndex))
}

private func quickSort<Element: Comparable>(_ array: inout [Element], _ low: Int, _ high: Int) {
  guard low < high else { return }
  
  let randomIndex = Int.random(in: low...high)
  array.swapAt(randomIndex, high)
  
  let partitionIndex = partition(&array, low, high)
  quickSort(&array, low, array.index(before: partitionIndex))
  quickSort(&array, array.index(after: partitionIndex), high)
}

private func partition<Element: Comparable>(_ array: inout [Element], _ low: Int, _ high: Int) -> Int {
  let partitionElement = array[high]
  var lowestIndex = low
  
  for currentIndex in low..<high {
    if partitionElement >= array[currentIndex] {
      array.swapAt(lowestIndex, currentIndex)
      lowestIndex += 1
    }
  }
  
  array.swapAt(lowestIndex, high)
  return lowestIndex
}

var a = [1, 3, 4, 5, 4, 3, 4, 3, 4, 3, 2, 3, 2, 2, 1]
quickSort(&a)
```

## Tree
```swift
class Node<Value> {
  let value: Value
  var children: [Node<Value>] = []
  
  init(_ value: Value) {
    self.value = value
  }
}

extension Node {
  func add(_ node: Node<Value>) {
    children.append(node)
  }
}

struct Queue<Value> {
  var elements: [Value] = []
  
  mutating func enqueue(_ value: Value) {
    elements.append(value)
  }
  
  mutating func dequeue() -> Value? {
    if elements.count > 0 {
      return elements.removeFirst()
    } else {
      return nil
    }
  }
}

func depthFirst<Value>(_ node: Node<Value>) {
  print(node.value)
  
  for child in node.children {
    depthFirst(child)
  }
}

func levelOrder<Value>(_ node: Node<Value>) {
  print(node.value)
  
  var queue = Queue<Node<Value>>()
  
  for child in node.children {
    queue.enqueue(child)
  }
  
  while let node = queue.dequeue() {
    print(node.value)
    for child in node.children {
      queue.enqueue(child)
    }
  }
}
https://stackoverflow.com/questions/37488316/partial-application-of-mutating-method-is-not-allowed
why you cant do node.child.forEach(queue.enqueue)
```


## Binary Tree
```swift
class Node<Value> {
  let value: Value
  var leftChild: Node<Value>?
  var rightChild: Node<Value>?
  
  init(value: Value) {
    self.value = value
  }
}

var tree: Node<Int> {
  let zero = Node(value: 0)
  let one = Node(value: 1)
  let five = Node(value: 5)
  let seven = Node(value: 7)
  let eight = Node(value: 8)
  let nine = Node(value: 9)
  seven.leftChild = one
  one.leftChild = zero
  one.rightChild = five
  seven.rightChild = nine
  nine.leftChild = eight
  return seven
}


func inOrder<Value>(_ node: Node<Value>?) {
  guard node != nil else { return }
  inOrder(node?.leftChild)
  print(node?.value ?? "")
  inOrder(node?.rightChild)
}

func preOrder<Value>(_ node: Node<Value>?) {
  guard node != nil else { return }
  print(node?.value ?? "")
  inOrder(node?.leftChild)
  inOrder(node?.rightChild)
}

func postOrder<Value>(_ node: Node<Value>?) {
  guard node != nil else { return }
  inOrder(node?.leftChild)
  inOrder(node?.rightChild)
  print(node?.value ?? "")
}

```




// Insertion

https://developer.apple.com/documentation/swift/range
Available when Bound conforms to Strideable and Bound.Stride conforms to SignedInteger.

//Using a Range as a Collection of Consecutive Values
//When a range uses integers as its lower and upper bounds, or any other type that conforms to the Strideable protocol with an integer stride, you can use that range in a for-in loop or with any sequence or collection method. The elements of the range are the consecutive values from its lower bound up to, but not including, its upper bound.

extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  mutating func insertionSort() {
    guard count > 1 else { return }

    for currentIndex in index(after: startIndex)..<endIndex {
      for shiftingIndex in (index(after: startIndex)...currentIndex).reversed() {
        let previousIndex = index(before: shiftingIndex)
        if self[previousIndex] > self[shiftingIndex] {
          swapAt(previousIndex, shiftingIndex)
        } else {
          break
        }
      }
    }
  }
}


## Bubble Sort

```swift
extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  
  /**
   Sorts a collection using a Bubble Sorting strategy.
   
   - Parameter areInIncreasingOrder: A predicate that returns true if its first argument should be ordered before its second argument; otherwise, false. If areInIncreasingOrder throws an error during the sort, the elements may be in a different order, but none will be lost.

   - Returns: A new sorted collection
   
   - Complexity: O(n^2), where n is the length of the collection. O(1) Space
   */
  mutating func bubbleSort(by areInIncreasingOrder: (Element, Element) throws -> Bool) rethrows {
    guard count > 1 else { return }
    
    for endingIndex in indices.reversed() {
      var noSwaps = true
      for currentIndex in startIndex..<endingIndex {
        let nextIndex = index(after: currentIndex)
        if try areInIncreasingOrder(self[nextIndex], self[currentIndex]) {
          swapAt(currentIndex, nextIndex)
          noSwaps = false
        }
      }
      if noSwaps { return }
    }
  }
}

var a = [7, 6, 8, 7, 6, 7, 6, 5, 6, 5, 4, 5, 4, 3, 4, 3, 2, 1]
a.bubbleSort(by: <)


[6, 5, 4, 5, 4, 3].sorted()
```


The sorting algorithm is not stable. A nonstable sort may change the relative order of elements that compare equal.

You can sort any mutable collection of elements that conform to the Comparable protocol by calling this method. Elements are sorted in ascending order.

The sorting algorithm is not stable. A nonstable sort may change the relative order of elements that compare equal.

Here’s an example of sorting a list of students’ names. Strings in Swift conform to the Comparable protocol, so the names are sorted in ascending order according to the less-than operator (<).

To sort the elements of your collection in descending order, pass the greater-than operator (>) to the sort(by:) method.

## Selection sort

```swift 

extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  
  mutating func selectionSort() {
    selectionSort(by: <)
  }
  
  mutating func selectionSort(by areInIncreasingOrder: (Element, Element) throws -> Bool) rethrows {
    guard count > 1 else { return }
    
    for currentIndex in startIndex..<index(before: endIndex) {
      var lowestValueIndex = currentIndex
      for swapIndex in index(after: currentIndex)..<endIndex {
        if try areInIncreasingOrder(self[swapIndex], self[lowestValueIndex]) {
          lowestValueIndex = swapIndex
        }
      }
      swapAt(lowestValueIndex, currentIndex)
    }
  }
}

var a = [9, 8, 9, 8, 7, 8, 7, 6, 7, 6, 5, 6, 5, 4, 3, 2, 1, 1]
a.selectionSort()

```

## insertion sort


```swift
// Insertion
// Best - O(n)
// Worst - O(n^2)
// Space - O(1)

extension MutableCollection where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  mutating func insertionSort() {
    insertionSort(by: <)
  }
  
  mutating func insertionSort(by areInIncreasingOrder: (Element, Element) throws -> Bool) rethrows {
    guard count > 1 else { return }
    
    for currentIndex in index(after: startIndex)..<endIndex {
      for shiftingIndex in (index(after: startIndex)...currentIndex).reversed() {
        let previousIndex = index(before: shiftingIndex)
        if try areInIncreasingOrder(self[shiftingIndex], self[previousIndex]) {
          swapAt(shiftingIndex, previousIndex)
        } else {
          break
        }
      }
    }
  }
}

var a = [9, 7, 8, 7, 6, 7, 6, 5, 6, 5, 4, 5, 4, 3, 4, 3, 2, 1]
a.insertionSort()
```




https://stackoverflow.com/questions/32968133/what-is-the-practical-use-of-nested-functions-in-swift

1
down vote
One use case is operations on recursive data structures.

Consider, for instance, this code for searching in binary search trees:

func get(_ key: Key) -> Value? {
    func recursiveGet(cur: Node) -> Value? {
        if cur.key == key {
            return cur.val
        } else if key < cur.key {
            return cur.left != nil ? recursiveGet(cur: cur.left!) : nil
        } else {
            return cur.right != nil ? recursiveGet(cur: cur.right!) : nil
        }
    }

    if let root = self.root {
        return recursiveGet(cur: root)
    } else {
        return nil
    }
}
Of course, you can transform the recursion into a loop, doing away with the nested function. I find recursive code often clearer than iterative variants, though. (Trade off vs. runtime cost!)

You could also define recursiveGet as (private) member outside of get but that would be bad design (unless recursiveGet is used in multiple methods).





enum SortOrder {
  case ascending
  case descending
}

extension MutableCollection
where Self: BidirectionalCollection, Element: Comparable, Index: Strideable, Index.Stride: SignedInteger {
  mutating func selectionSort(in order: SortOrder) {
    switch order {
    case .ascending:   selectionSort(by: <)
    case .descending:  selectionSort(by: >)
    }
  }
  
  mutating func selectionSort(by areInIncreasingOrder: (Element, Element) throws -> Bool) rethrows {
    guard count > 1 else { return }
    
    for currentIndex in startIndex..<index(before: endIndex) {
      var lowestValueIndex = currentIndex
      for swapIndex in index(after: currentIndex)..<endIndex {
        if try areInIncreasingOrder(self[swapIndex], self[lowestValueIndex]) {
          lowestValueIndex = swapIndex
        }
      }
      swapAt(currentIndex, lowestValueIndex)
    }
  }
}

var a = [7, 6, 7, 6, 5, 6, 5, 4, 5, 4, 3, 4, 3, 2, 1]
a.selectionSort(in: .ascending)


fibs with anysequence and anyiterator

let fibs = AnySequence { () -> AnyIterator<Int> in
  var state = (0, 1)
  
  return AnyIterator {
    defer {
      state = (state.1, state.0 + state.1)
    }
    return state.0
  }
}

for fib in fibs.prefix(10) {
  print(fib)
}









https://medium.com/retailmenot-engineering/down-the-swift-associated-type-rabbit-hole-1f41ee6ceaf4

Take in two Arrays (or ArraySlices), base and newElems as parameters, and return a new array
The output array should be made by adding each element of newElems to base, but only if it wasn’t already in base
All input and output array should be the same type T



https://www.objc.io/blog/2018/03/20/lazy-infinite-sequences/






## implementing map

https://stackoverflow.com/questions/40152826/swift-collection-underestimatecount-usage

extension Sequence {
  func mapped<T>(_ transform: (Element) throws -> T) rethrows -> [T] {
    let initialCapacity = underestimatedCount
    var result = ContiguousArray<T>()
    result.reserveCapacity(initialCapacity)
    
    var iterator = makeIterator()
    
    for _ in 0..<initialCapacity {
      result.append(try transform(iterator.next()!))
    }
    
    while let element = iterator.next() {
      result.append(try transform(element))
    }
    return Array(result)
  }








https://medium.com/retailmenot-engineering/down-the-swift-associated-type-rabbit-hole-1f41ee6ceaf4

extension Sequence where Element: Hashable {
  func unique() -> [Element] {
    var elementSet = Set<Element>()
    return filter {
      if elementSet.contains($0) {
        return false
      } else {
        elementSet.insert($0)
        return true
      }
    }
  }
  
  func notContains(_ element: Element) -> Bool {
    return !contains(element)
  }
}

func merge<T: Collection, U: Collection>(_ base: T, withNewElems newElems: U) -> [T.Element]
  where T.Element: Hashable, T.Element == U.Element {
    let uniqueElems = newElems.filter(base.notContains)
    return Array(base) + uniqueElems
}

merge([1, 2, 3, 4, 4, 4, 4], withNewElems: [6, 5, 4, 5, 4, 5, 3])



traverse a matrix
reverse a utf8 string

http://omarrr.com/underscores-in-apples-swift-numbers/
https://www.raywenderlich.com/1220-swift-tutorial-initialization-in-depth-part-1-2


https://stackoverflow.com/questions/3255/big-o-how-do-you-calculate-approximate-it

https://www.khanacademy.org/computing/computer-science/algorithms/intro-to-algorithms/v/what-are-algorithms

https://www.youtube.com/watch?v=y2b94AxPlF8




// Quick Sort
// Best - O(n log n)
// Worst - O(n^2)
// Space - O(log n)
// Unstable


extension Array where Element: Comparable {
    mutating func quickSort() {
        quickSort(startIndex, index(before: endIndex))
    }
    
    private mutating func quickSort(_ low: Index, _ high: Index, by ordered: (Element, Element) -> Bool = (<=)) {
        guard low < high else { return }
        
        let randomIndex = Int.random(in: low...high)
        swapAt(randomIndex, high)
        
        let partitionIndex = partition(low, high, by: ordered)
        quickSort(low, index(before: partitionIndex))
        quickSort(index(after: partitionIndex), high)
    }
    
    private mutating func partition(_ start: Index, _ end: Index, by ordered: (Element, Element) -> Bool = (<=)) -> Index {
        let partitionElement = self[end]
        var startIndex = start
        
        for currentIndex in start..<end {
            if ordered(self[currentIndex], partitionElement) {
                swapAt(currentIndex, startIndex)
                startIndex = index(after: startIndex)
            }
        }
        
        swapAt(startIndex, end)
        return startIndex
    }
}

var a = [1, 3, 7, 6, 5, 8, 7, 9, 8, 7, 6, 5, 6, 5, 4, 3]
a.quickSort()
