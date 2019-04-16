# Trie

```swift
struct Trie<CollectionType: Collection> where CollectionType.Element: Hashable {
    typealias Node = TrieNode<CollectionType.Element>
    var root = Node(key: nil, parent: nil)
}
```

## Insert
* Time: ```O(k)``` where ```k``` is the length of the input collection
* Space: ```O(1)```

```swift
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
}
```

## Remove
* Time: ```O(k)``` where ```k``` is the length of the input collection
* Space: ```O(1)```

```swift
extension Trie {
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
}
```

## Contains
* Time: ```O(k)``` where ```k``` is the length of the input collection
* Space: ```O(1)```

```swift
extension Trie {
    func contains(_ collection: CollectionType) -> Bool {
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
```

## Collections Starting with Prefix
* Time: ```O(k*m)``` where ```k``` is the longest collection matching the prefix and ```m``` is number of collections that match the prefix
* Space: ```O(h)``` where ```h``` is the height of the tree

```swift
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

# Trie Node
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
```
