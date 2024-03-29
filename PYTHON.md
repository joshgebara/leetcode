# Leetcode Python Solutions

## 953. Verifying an Alien Dictionary
```python
class Solution:    
    def isAlienSorted(self, words: List[str], order: str) -> bool:
        map = {}
        
        for i, char in enumerate(order):
            map[char] = i
        
        for index in range(1, len(words)):
            prevWord = words[index - 1]
            word = words[index]
            
            if not self.inOrder(prevWord, word, map):
                return False
        
        return True
    
    def inOrder(self, word1, word2, map):
        for i, j in zip(word1, word2):
            if map[i] == map[j]:
                continue
            
            return map[i] < map[j]
        
        return len(word1) <= len(word2)
```

## 67. Add Binary
```python
class Solution:
    def addBinary(self, a: str, b: str) -> str:
        result = []
        
        carry = 0
        
        i = len(a) - 1
        j = len(b) - 1
        
        while i >= 0 or j >= 0:
            aNum = int(a[i]) if i >= 0 else 0
            bNum = int(b[j]) if j >= 0 else 0
            
            sum = aNum + bNum + carry
            
            result.append(str(sum % 2))
            carry = sum // 2
            
            i -= 1
            j -= 1
            
        if (carry > 0):
            result.append(str(carry))
            
        result.reverse()
        return ''.join(result)
```

## 680. Valid Palindrome II
```python
class Solution:
    def validPalindrome(self, s: str) -> bool:
        def _validPalindrome(left, right, deletes) -> bool:
            while left < right:
                if (s[left] != s[right]):
                    if (deletes):
                        deleteLeft = _validPalindrome(left + 1, right, deletes - 1)
                        deleteRight = _validPalindrome(left, right - 1, deletes - 1)
                        return deleteLeft or deleteRight
                    else:
                        return False
                else:
                    left += 1
                    right -= 1
            return True
        
        return _validPalindrome(0, len(s) - 1, 1)
```

## 415. Add Strings
```python
class Solution:
    def addStrings(self, num1: str, num2: str) -> str:
        result = []
        
        carry = 0
        
        i = len(num1) - 1
        j = len(num2) - 1
        
        while i >= 0 or j >= 0:
            iNum = int(num1[i]) if i >= 0 else 0
            jNum = int(num2[j]) if j >= 0 else 0
            
            sum = iNum + jNum + carry
            
            result.append(str(sum % 10))
            carry = sum // 10
            
            i -= 1
            j -= 1
            
        if carry > 0:
            result.append(str(carry))
            
        result.reverse()
        return ''.join(result)
```

## 278. First Bad Version
```python
# The isBadVersion API is already defined for you.
# @param version, an integer
# @return an integer
# def isBadVersion(version):

class Solution:
    def firstBadVersion(self, n):
        """
        :type n: int
        :rtype: int
        """
        left = 1
        right = n
        
        while (left < right):
            mid = (right - left) // 2 + left
            
            if isBadVersion(mid):
                right = mid
            else:
                left = mid + 1
                
        return left
```

## 283. Move Zeroes
```python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        i = 0
        for j in range(len(nums)):
            if (nums[j] != 0):
                nums[i], nums[j] = nums[j], nums[i]
                i += 1
```

## 543. Diameter of Binary Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def diameterOfBinaryTree(self, root: TreeNode) -> int:
        def _diameterOfBinaryTree(root: TreeNode) -> int:
            if root is None:
                return 0

            left = _diameterOfBinaryTree(root.left)
            right = _diameterOfBinaryTree(root.right)
            
            nonlocal diameter
            diameter = max(diameter, left + right)
            
            return 1 + max(left, right)
        
        diameter = 0
        _diameterOfBinaryTree(root)
        return diameter
```

## 125. Valid Palindrome
```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        left = 0
        right = len(s) - 1
        
        while left < right:
            leftChar = s[left].lower()            
            if not leftChar.isalnum():
                left += 1
                continue
            
            rightChar = s[right].lower()
            if not rightChar.isalnum():
                right -= 1
                continue
                
            if leftChar == rightChar:
                left += 1
                right -= 1
                continue
            
            return False
            
        return True
        
```

## 88. Merge Sorted Array
```python
class Solution:
    def merge(self, nums1: List[int], m: int, nums2: List[int], n: int) -> None:
        """
        Do not return anything, modify nums1 in-place instead.
        """
        write = m + n - 1
        read1 = m - 1
        read2 = n - 1
        
        while read1 >= 0 and read2 >= 0:
            if nums1[read1] < nums2[read2]:
                nums1[write] = nums2[read2]
                read2 -= 1
            else:
                nums1[write] = nums1[read1]
                read1 -= 1
            
            write -= 1
        
        while read2 >= 0:
            nums1[write] = nums2[read2]
            read2 -= 1
            write -= 1
        
```

## 1. Two Sum
```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        map = {}
        
        for i, num in enumerate(nums):
            candidate = target - num
            
            if candidate in map:
                return [map[candidate], i]
            
            map[num] = i
```

## 938. Range Sum of BST
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rangeSumBST(self, root: TreeNode, low: int, high: int) -> int:
        if root is None:
            return 0
        
        if root.val < low:
            return self.rangeSumBST(root.right, low, high)
        
        if high < root.val:
            return self.rangeSumBST(root.left, low, high)
        
        left = self.rangeSumBST(root.left, low, high)
        right = self.rangeSumBST(root.right, low, high)
        
        return left + right + root.val
```

## 257. Binary Tree Paths
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def binaryTreePaths(self, root: TreeNode) -> List[str]:
        def dfs(root):
            if root is None:
                return
            
            path.append(str(root.val))
            
            if root.left is None and root.right is None:
                paths.append('->'.join(path))
                path.pop()
                return
            
            dfs(root.left)
            dfs(root.right)
            
            path.pop()
        
        paths = []
        path = []
        dfs(root)
        return paths
```

## 349. Intersection of Two Arrays
```python
class Solution:
    def intersection(self, nums1: List[int], nums2: List[int]) -> List[int]:
        return set(nums1) & set(nums2)
```

## 121. Best Time to Buy and Sell Stock
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        minPrice = prices[0]
        maxProfit = 0
        
        for price in prices[1:]:
            minPrice = min(minPrice, price)
            maxProfit = max(maxProfit, price - minPrice)
            
        return maxProfit
```

## 824. Goat Latin
```python
class Solution:
    def toGoatLatin(self, sentence: str) -> str:
        def startsWithVowel(word):
            vowels = { 'a', 'e', 'i', 'o', 'u' }
            return word[0].lower() in vowels
            
        result = []
        
        for i, word in enumerate(sentence.split(' ')):
            translatedWord = []
            
            if startsWithVowel(word):
                translatedWord.append(word)
            else:
                translatedWord.append(word[1:])
                translatedWord.append(word[:1])
                
            translatedWord.append("ma")
            translatedWord.append("a" * (i + 1))
            
            result.append(''.join(translatedWord))
            
        return ' '.join(result)
```

## 246. Strobogrammatic Number
```python
class Solution:
    def isStrobogrammatic(self, num: str) -> bool:
        dict = { '0': '0', '1': '1', '6': '9', '8': '8', '9': '6' }
        
        left = 0
        right = len(num) - 1
        
        while left <= right:
            if num[left] not in dict or dict[num[left]] != num[right]:
                return False
            
            left += 1
            right -= 1
            
        return True
```

## 463. Island Perimeter
```python
class Solution:
    def islandPerimeter(self, grid: List[List[int]]) -> int:
        dirs = [[1, 0], [0, 1], [-1, 0], [0, -1]]
        perimeter = 0
        
        for row in range(len(grid)):
            for col in range(len(grid[0])):
                if grid[row][col]:
                    perimeter += 4
                    
                    for [deltaRow, deltaCol] in dirs:
                        nextRow = row + deltaRow
                        nextCol = col + deltaCol
                        
                        if 0 <= nextRow < len(grid) and 0 <= nextCol < len(grid[0]) and grid[nextRow][nextCol]:
                            perimeter -= 1
                            
        return perimeter
        
```

## 350. Intersection of Two Arrays II
```python
from collections import Counter

class Solution:
    def intersect(self, nums1: List[int], nums2: List[int]) -> List[int]:
        counter = Counter(nums1)
        
        result = []
        
        for num in nums2:
            if counter[num] > 0:
                result.append(num)
                counter[num] -= 1
                
        return result
```

## 266. Palindrome Permutation
```python
from collections import Counter

class Solution:
    def canPermutePalindrome(self, s: str) -> bool:
        counter = Counter(s)
        
        oddCount = 0
        
        for key in counter:
            if counter[key] & 1:
                oddCount += 1
                
                if oddCount > 1:
                    return False
                
        return True
```

## 977. Squares of a Sorted Array
```python
class Solution:
    def sortedSquares(self, nums: List[int]) -> List[int]:
        result = [0] * len(nums)
        write = len(nums) - 1
        
        left = 0
        right = len(nums) - 1
        
        while left <= right:
            leftSquare = nums[left] ** 2
            rightSquare = nums[right] ** 2
            
            if leftSquare < rightSquare:
                result[write] = rightSquare
                right -= 1
            else:
                result[write] = leftSquare
                left += 1
                
            write -= 1
            
        return result
```

## 53. Maximum Subarray
```python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        localMax = nums[0]
        globalMax = nums[0]
        
        for num in nums[1:]:
            localMax = max(num, num + localMax)
            globalMax = max(globalMax, localMax)
        
        return globalMax
```

## 1047. Remove All Adjacent Duplicates In String
```python
class Solution:
    def removeDuplicates(self, s: str) -> str:
        stack = []
        
        for char in s:
            if stack and stack[-1] == char:
                stack.pop()
                continue
                
            stack.append(char)
            
        return ''.join(stack)
```

## 206. Reverse Linked List
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        prev = None
        next = None
        
        while head:
            next = head.next
            head.next = prev
            prev = head
            head = next
            
        return prev
```

## 21. Merge Two Sorted Lists
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:
        dummy = ListNode()
        curr = dummy
        
        while l1 and l2:
            if l1.val < l2.val:
                curr.next = l1
                l1 = l1.next
            else:
                curr.next = l2
                l2 = l2.next
            curr = curr.next
        
        if l1:
            curr.next = l1
        else:
            curr.next = l2
        
        return dummy.next
```

## 111. Minimum Depth of Binary Tree
### DFS - Recursive
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
import math

class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        def dfs(root):
            if not root:
                return math.inf
            
            if not root.left and not root.right:
                return 1
            
            return 1 + min(dfs(root.left), dfs(root.right))
        
        if not root:
            return 0
        
        return dfs(root)
```

### BFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        depth = 1
        
        queue = deque([root])
        while queue:
            size = len(queue)
            for _ in range(size):
                node = queue.popleft()
                
                if not node.left and not node.right:
                    return depth
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                    
            depth += 1
            
        return depth
```

### DFS - Iterative Preorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        curr = root
        stack = []
        
        minDep = math.inf
        currDep = 1
        
        while curr or stack:
            while curr:
                if not curr.left and not curr.right:
                    minDep = min(minDep, currDep)
                
                stack.append([curr, currDep])
                currDep += 1
                curr = curr.left
                
            node, depth = stack.pop()
            currDep = depth
            
            if node.right:
                currDep += 1
                curr = node.right
                
                
        return minDep
```

### DFS - Iterative Inorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        curr = root
        stack = []
        
        minDep = math.inf
        currDep = 1
        
        while curr or stack:
            while curr:
                stack.append([curr, currDep])
                currDep += 1
                curr = curr.left
                
            node, depth = stack.pop()
            currDep = depth
            
            if not node.left and not node.right:
                minDep = min(minDep, currDep)
            
            if node.right:
                currDep += 1
                curr = node.right
                
                
        return minDep
```

### DFS - Iterative Postorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        curr = root
        stack = []
        
        minDep = math.inf
        currDep = 1
        
        while curr or stack:
            while curr:
                stack.append([curr, currDep])
                currDep += 1
                curr = curr.left
            
            if stack[-1][0].right:
                curr = stack[-1][0].right
                continue
            
            node, depth = stack.pop()
            currDep = depth
            
            if not node.left and not node.right:
                minDep = min(minDep, currDep)
            
            while stack and stack[-1][0].right is node:
                node, depth = stack.pop()
                currDep = depth
                
                if not node.left and not node.right:
                    minDep = min(minDep, currDep)
                    
        return minDep
```



## 1213. Intersection of Three Sorted Arrays
```python
class Solution:
    def arraysIntersection(self, arr1: List[int], arr2: List[int], arr3: List[int]) -> List[int]:
        result = []
        
        i = 0
        j = 0
        k = 0
        
        while (i < len(arr1) and j < len(arr2) and k < len(arr3)):
            if arr1[i] == arr2[j] == arr3[k]:
                result.append(arr1[i])
                i += 1
                j += 1
                k += 1
                continue
            
            groupMax = max(arr1[i], arr2[j], arr3[k])
            if arr1[i] != groupMax:
                i += 1
                
            if arr2[j] != groupMax:
                j += 1
                continue
                
            if arr3[k] != groupMax:
                k += 1
                continue
                
        return result
```

## 724. Find Pivot Index
```python
class Solution:
    def pivotIndex(self, nums: List[int]) -> int:
        totalSum = sum(nums)
        
        leftSum = 0
        for i, num in enumerate(nums):
            rightSum = totalSum - leftSum - num
            
            if leftSum == rightSum:
                return i
            
            leftSum += num
            
        return -1
```

## 387. First Unique Character in a String
```python
from collections import Counter

class Solution:
    def firstUniqChar(self, s: str) -> int:
        counter = Counter(s)
        
        for i, char in enumerate(s):
            if (counter[char] == 1):
                return i
            
        return -1
```

## 637. Average of Levels in Binary Tree
### DFS - Recursive
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def averageOfLevels(self, root: Optional[TreeNode]) -> List[float]:
        def dfs(root, level):
            nonlocal averages
            
            if not root:
                return
            
            if level not in averages:
                averages[level] = [0, 0]
                
            averages[level] = [averages[level][0] + root.val, averages[level][1] + 1]
            
            dfs(root.left, level + 1)
            dfs(root.right, level + 1)
            
        averages = {}
        dfs(root, 0)
        
        result = []
        for sum, count in averages.values():
            result.append(sum / count)
        
        return result
```

### DFS - Iterative Preorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

from collections import deque

class Solution:
    def averageOfLevels(self, root: Optional[TreeNode]) -> List[float]:
        result = []

        curr = root
        stack = []
        depth = 0

        while curr or stack:
            while curr:
                if depth >= len(result):
                    result.append([0, 0])

                result[depth][0] += curr.val
                result[depth][1] += 1

                stack.append([curr, depth])
                curr = curr.left
                depth += 1

            node, d = stack.pop()
            depth = d

            if node.right:
                curr = node.right
                depth += 1
                
        return map(lambda x: x[0] / x[1], result)
```

### DFS - Iterative Postorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

from collections import deque

class Solution:
    def averageOfLevels(self, root: Optional[TreeNode]) -> List[float]:
        levelsDict = {}

        curr = root
        stack = []
        depth = 0
        maxDepth = 0

        while curr or stack:
            while curr:
                stack.append([curr, depth])
                curr = curr.left
                depth += 1
                maxDepth = max(maxDepth, depth)
                
            if stack[-1][0].right:
                curr = stack[-1][0].right
                continue

            node, d = stack.pop()
            depth = d
            
            if depth not in levelsDict:
                levelsDict[depth] = [0, 0]
            
            levelsDict[depth][0] += node.val
            levelsDict[depth][1] += 1
            
            while stack and stack[-1][0].right is node:
                node, d = stack.pop()
                depth = d
                
                if depth not in levelsDict:
                    levelsDict[depth] = [0, 0]
                
                levelsDict[depth][0] += node.val
                levelsDict[depth][1] += 1
                
        result = [0] * maxDepth
        
        for i in range(maxDepth):
            result[i] = levelsDict[i][0] / levelsDict[i][1]
            
        return result
```

### BFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def averageOfLevels(self, root: TreeNode) -> List[float]:
        queue = deque([root])
        
        levels = []
        while queue:
            sum = 0
            size = len(queue)
            
            for i in range(size):
                node = queue.popleft()
                
                sum += node.val
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            
            levels.append(sum / size)
            
        return levels
```

### DFS - Morris Inorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def averageOfLevels(self, root: Optional[TreeNode]) -> List[float]:
        def morris(root):
            nonlocal levelsDict
            nonlocal maxDepth
            
            curr = root
            depth = 0
            
            while curr:
                if not curr.left:
                    curr.depth = depth
                    maxDepth = max(maxDepth, depth)
                    depth += 1
                    
                    if curr.depth not in levelsDict:
                        levelsDict[curr.depth] = [0, 0]
                    levelsDict[curr.depth][0] += curr.val
                    levelsDict[curr.depth][1] += 1
                    
                    curr = curr.right
                else:
                    pred = curr.left
                    
                    while pred.right and pred.right is not curr:
                        pred = pred.right
                        
                    if pred.right is curr:
                        depth = curr.depth
                        maxDepth = max(maxDepth, depth)
                        depth += 1
                        
                        if curr.depth not in levelsDict:
                            levelsDict[curr.depth] = [0, 0]
                        levelsDict[curr.depth][0] += curr.val
                        levelsDict[curr.depth][1] += 1
                        
                        pred.right = None
                        curr = curr.right
                    else:
                        curr.depth = depth
                        depth += 1
                        
                        pred.right = curr
                        curr = curr.left
        
        levelsDict = {}
        maxDepth = 0
        
        morris(root)
        
        result = []
        for level in range(maxDepth + 1):
            result.append(levelsDict[level][0] / levelsDict[level][1])
            
        return result
```

## 122. Best Time to Buy and Sell Stock II
```python
class Solution:
    def maxProfit(self, prices: List[int]) -> int:
        profit = 0
        
        for i in range(1, len(prices)):
            profit += max(prices[i] - prices[i - 1], 0)
            
        return profit
```

## 896. Monotonic Array
```python
class Solution:
    def isMonotonic(self, nums: List[int]) -> bool:
        increasing = True
        decreasing = True
        
        for i in range(len(nums) - 1):
            if nums[i] == nums[i + 1]:
                continue
                
            if nums[i] < nums[i + 1]:
                decreasing = False
            
            if nums[i] > nums[i + 1]:
                increasing = False
            
        return increasing or decreasing
```

## 461. Hamming Distance
```python
class Solution:
    def hammingDistance(self, x: int, y: int) -> int:
        diff = x ^ y
        
        count = 0
        
        while diff:
            count += 1
            diff &= diff - 1
            
        return count
```

## 367. Valid Perfect Square
```python
class Solution:
    def isPerfectSquare(self, num: int) -> bool:
        left = 1
        right = num // 2 + 1
        
        while left <= right:
            mid = (right - left) // 2 + left
            
            square = mid ** 2
            
            if square == num:
                return True
            elif square < num:
                left = mid + 1
            else:
                right = mid - 1
            
        return False
            
```

## 268. Missing Number
### Gauss Formula
```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        n = len(nums)
        expectedSum = n * (n + 1) // 2
        actualSum = sum(nums)
        return expectedSum - actualSum
```

### XOR
```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        missing = 0
        for num in range(len(nums) + 1):
            missing ^= num
        
        for num in nums:
            missing ^= num
            
        return missing
```

## 1460. Make Two Arrays Equal by Reversing Sub-arrays
```python
from collections import Counter

class Solution:
    def canBeEqual(self, target: List[int], arr: List[int]) -> bool:
        return Counter(target) == Counter(arr)
```

## 1108. Defanging an IP Address
```python
class Solution:
    def defangIPaddr(self, address: str) -> str:
        result = []
        
        for char in address:
            if (char == '.'):
                result.append('[.]')
            else:
                result.append(char)
        
        return ''.join(result)
```

## 108. Convert Sorted Array to Binary Search Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sortedArrayToBST(self, nums: List[int]) -> Optional[TreeNode]:
        def toBST(left, right):
            if left > right:
                return None
            
            mid = (right - left) // 2 + left
            
            node = TreeNode(nums[mid])
            
            node.left = toBST(left, mid - 1)
            node.right = toBST(mid + 1, right)
            
            return node
        
        return toBST(0, len(nums) - 1)
```

## 226. Invert Binary Tree
### DFS - Recursive Preorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if root is None:
            return root
        
        root.left, root.right = root.right, root.left
        
        self.invertTree(root.left)
        self.invertTree(root.right)
        
        return root
```

### BFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

from collections import deque

class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def bfs(root):
            if not root:
                return
            
            queue = deque([root])
            
            while queue:
                node = queue.popleft()
                
                temp = node.left
                node.left = node.right
                node.right = temp
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            
        bfs(root)
        return root
```

### DFS - Iterative Preorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def dfs(root):
            curr = root
            stack = []
            
            while curr or stack:
                while curr:
                    temp = curr.left
                    curr.left = curr.right
                    curr.right = temp
                    
                    stack.append(curr)
                    curr = curr.left
                
                node = stack.pop()
                
                curr = node.right
                    
        dfs(root)
        return root
```

### DFS - Iterative Postorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def dfs(root):
            curr = root
            stack = []
            
            while curr or stack:
                while curr:
                    stack.append(curr)
                    curr = curr.left
                    
                if stack[-1].right:
                    curr = stack[-1].right
                    continue
                
                node = stack.pop()
                temp = node.left
                node.left = node.right
                node.right = temp
                
                while stack and stack[-1].right is node:
                    node = stack.pop()
                    temp = node.left
                    node.left = node.right
                    node.right = temp
        
        dfs(root)
        return root
```


## 344. Reverse String
```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        """
        Do not return anything, modify s in-place instead.
        """
        left = 0
        right = len(s) - 1
        
        while left < right:
            s[left], s[right] = s[right], s[left]
            
            left += 1
            right -= 1
```

## 155. Min Stack
```python
class MinStack:

    def __init__(self):
        """
        initialize your data structure here.
        """
        self.stack = []
        self.minStack = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        
        if not self.minStack or self.minStack[-1] >= val:
            self.minStack.append(val)

    def pop(self) -> None:
        val = self.stack.pop()
        
        if self.minStack[-1] == val:
            self.minStack.pop()

    def top(self) -> int:
        return self.stack[-1]
        

    def getMin(self) -> int:
        return self.minStack[-1]


# Your MinStack object will be instantiated and called as such:
# obj = MinStack()
# obj.push(val)
# obj.pop()
# param_3 = obj.top()
# param_4 = obj.getMin()
```

## 100. Same Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        if not p and not q:
            return True
        
        if not p or not q:
            return False
        
        if p.val != q.val:
            return False
        
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

## 104. Maximum Depth of Binary Tree
### DFS - Recursive Postorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        left = self.maxDepth(root.left)
        right = self.maxDepth(root.right)
        return 1 + max(left, right)
```

### BFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        def bfs(root):
            depth = 0
            queue = deque([root])
            
            while queue:
                size = len(queue)
                for _ in range(size):
                    node = queue.popleft()
                    
                    if node.left:
                        queue.append(node.left)
                    if node.right:
                        queue.append(node.right)
                
                depth += 1
                
            return depth
        
        if not root:
            return 0
        
        return bfs(root)
```

### DFS - Iterative Preorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

from collections import deque

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        def dfs(root):
            curr = root
            stack = []
            
            maxDep = 0
            currDep = 0
            
            while curr or stack:
                while curr:
                    stack.append([curr, currDep])
                    currDep += 1
                    maxDep = max(maxDep, currDep)
                    
                    curr = curr.left
                    
                node, dep = stack.pop()                
                currDep = dep
                
                if node.right:
                    curr = node.right
                    currDep += 1
                    maxDep = max(maxDep, currDep)
                    
            return maxDep
        
        if not root:
            return 0
        
        return dfs(root)
```

### DFS - Iterative Postorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

from collections import deque

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        def dfs(root):
            curr = root
            stack = []
            
            maxDep = 0
            currDep = 0
            
            while curr or stack:
                while curr:
                    stack.append([curr, currDep])
                    currDep += 1
                    maxDep = max(maxDep, currDep)
                    curr = curr.left
                    
                if stack[-1][0].right:
                    curr = stack[-1][0].right
                    continue
                
                node, dep = stack.pop()
                currDep = dep
                
                while stack and stack[-1][0].right is node:
                    node, dep = stack.pop()
                    currDep = dep
                    
            return maxDep
        
        if not root:
            return 0
        
        return dfs(root)
```

## 70. Climbing Stairs
### Top Down DP
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        def _climbStairs(n: int):
            if n <= 1:
                return 1
            if n in memo:
                return memo[n]

            memo[n] = _climbStairs(n - 1) + _climbStairs(n - 2)
            return memo[n]
        
        memo = {}
        return _climbStairs(n)
```
### Bottom Up DP
```python
class Solution:
    def climbStairs(self, n: int) -> int:
        dp1 = 1
        dp2 = 1
        
        for i in range(2, n + 1):
            next = dp1 + dp2
            dp2 = dp1
            dp1 = next
        
        return dp1
```

## 993. Cousins in Binary Tree
### DFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isCousins(self, root: Optional[TreeNode], x: int, y: int) -> bool:
        def dfs(root, parent, depth):
            nonlocal xParent
            nonlocal yParent
            nonlocal xDepth
            nonlocal yDepth
            
            if not root:
                return False
            
            if root.val == x:
                xParent = parent
                xDepth = depth
            elif root.val == y:
                yParent = parent
                yDepth = depth
                
            if xParent and yParent and xParent is not yParent and xDepth == yDepth:
                return True
            
            return dfs(root.left, root, depth + 1) or dfs(root.right, root, depth + 1)
            
        xParent = None
        yParent = None
        xDepth = 0
        yDepth = 0
        
        return dfs(root, None, 0)
```

### BFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def isCousins(self, root: Optional[TreeNode], x: int, y: int) -> bool:
        queue = deque([[root, None]])
        
        while queue:
            size = len(queue)
            
            parentX = None
            parentY = None
            
            for _ in range(size):
                node, parent = queue.popleft()
                
                if node.val == x:
                    parentX = parent
                elif node.val == y:
                    parentY = parent
                
                if node.left:
                    queue.append([node.left, node])
                if node.right:
                    queue.append([node.right, node])
            
            if parentX and parentY and parentX is not parentY:
                return True
            
        return False
```

## 617. Merge Two Binary Trees
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def mergeTrees(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root1 and not root2:
            return None
        if not root1:
            return root2
        if not root2:
            return root1
        
        node = TreeNode(root1.val + root2.val)
        
        node.left = self.mergeTrees(root1.left, root2.left)
        node.right = self.mergeTrees(root1.right, root2.right)
        
        return node
```

## 383. Ransom Note
```python
from collections import Counter

class Solution:
    def canConstruct(self, ransomNote: str, magazine: str) -> bool:
        letters = Counter(magazine)
        
        for char in ransomNote:
            if not letters.get(char):
                return False
            
            letters[char] -= 1
            
        return True
                
```

## 9. Palindrome Number
```python
class Solution:
    def isPalindrome(self, x: int) -> bool:
        if x < 0:
            return False
        if 0 <= x <= 9:
            return True
        if x % 10 == 0:
            return False
        
        reversedNum = 0
        num = x
        while x:
            digit = x % 10
            
            reversedNum *= 10
            reversedNum += digit
            
            x //= 10
        
        return reversedNum == num
```

## 989. Add to Array-Form of Integer
```python
class Solution:
    def addToArrayForm(self, num: List[int], k: int) -> List[int]:
        result = []
        
        carry = k
        
        for digit in num[::-1]:
            sum = carry + digit
            
            result.append(sum % 10)
            carry = sum // 10
            
        while carry:
            result.append(carry % 10)
            carry //= 10
            
        result.reverse()
        return result
```

## 1099. Two Sum Less Than K
```python
class Solution:
    def twoSumLessThanK(self, nums: List[int], k: int) -> int:
        nums.sort()
        
        maxSum = -1
        
        left = 0
        right = len(nums) - 1
        
        while (left < right):
            sum = nums[left] + nums[right]
            
            if sum < k:
                maxSum = max(maxSum, sum)
                left += 1
            else:
                right -= 1
        
        return maxSum
```

## 997. Find the Town Judge
```python
class Solution:
    def findJudge(self, n: int, trust: List[List[int]]) -> int:
        degrees = {}
        
        for person in range(1, n + 1):
            degrees[person] = { 'in': 0, 'out': 0 }
        
        for a, b in trust:
            degrees[a]['out'] += 1
            degrees[b]['in'] += 1
            
        for person in degrees.keys():
            if degrees[person]['in'] == n - 1 and degrees[person]['out'] == 0:
                return person
        
        return -1
```

## 412. Fizz Buzz
```python
class Solution:
    def fizzBuzz(self, n: int) -> List[str]:
        values = [(3, 'Fizz'), (5, 'Buzz')]
        
        result = []
        
        for num in range(1, n + 1):
            curr = ''
            
            for divisor, value in values:
                if num % divisor == 0:
                    curr += value
                    
            if not curr:
                curr = str(num)
                
            result.append(curr)
        
        return result
```

## 509. Fibonacci Number
### Top Down DP
```python
class Solution:
    def fib(self, n: int) -> int:
        def _fib(n):
            if n in memo:
                return memo[n]
            if n <= 1:
                return n
            memo[n] = _fib(n - 1) + _fib(n - 2)
            return memo[n]
        
        memo = {}
        return _fib(n)
```

### Bottom Up DP
```python
class Solution:
    def fib(self, n: int) -> int:
        if n <= 1:
            return n
        
        numBack2 = 0
        numBack1 = 1
        
        for i in range(2, n + 1):
            numBack0 = numBack1 + numBack2
            numBack2 = numBack1
            numBack1 = numBack0
            
        return numBack1
```

## 1614. Maximum Nesting Depth of the Parentheses
```python
class Solution:
    def maxDepth(self, s: str) -> int:
        maxNestingDepth = 0
        currNestingdepth = 0
        
        for char in s:
            if char == '(':
                currNestingdepth += 1
                maxNestingDepth = max(maxNestingDepth, currNestingdepth)
            elif char == ')':
                currNestingdepth -= 1
                
        return maxNestingDepth
```

## 872. Leaf-Similar Trees
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def leafSimilar(self, root1: Optional[TreeNode], root2: Optional[TreeNode]) -> bool:
        return self.getLeaves(root1) == self.getLeaves(root2)
    
    def getLeaves(self, root):
        def dfs(root):
            if not root:
                return
            
            if not root.left and not root.right:
                leaves.append(root.val)
            
            dfs(root.left)
            dfs(root.right)
            
        leaves = []
        dfs(root)
        return leaves
```

## 141. Linked List Cycle
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: ListNode) -> bool:
        fast = head
        slow = head
        
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            
            if fast == slow:
                return True
            
        return False
```

## 704. Binary Search
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1
        
        while (left <= right):
            mid = (right - left) // 2 + left
            
            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                left = mid + 1
            else:
                right = mid - 1
                
        return -1
```

## 832. Flipping an Image
```python
class Solution:
    def flipAndInvertImage(self, image: List[List[int]]) -> List[List[int]]:
        def flipAndInvertRow(row):
            left = 0
            right = len(row) - 1
            
            while left <= right:
                if left == right:
                    row[left] ^= 1
                    break
                
                row[left], row[right] = row[right], row[left]
                
                row[left] ^= 1
                row[right] ^= 1
                
                left += 1
                right -=1
        
        for row in image:
            flipAndInvertRow(row)
            
        return image
```

## 217. Contains Duplicate
```python
class Solution:
    def containsDuplicate(self, nums: List[int]) -> bool:
        return len(set(nums)) != len(nums)
```

## 118. Pascal's Triangle
```python
class Solution:
    def generate(self, numRows: int) -> List[List[int]]:
        triangle = [[1]]
        
        for row in range(1, numRows):
            prevRow = triangle[-1]
            nextRow = [1]
            
            for prevCol in range(1, len(prevRow)):
                nextRow.append(prevRow[prevCol - 1] + prevRow[prevCol])
                
            nextRow.append(1)
            triangle.append(nextRow)
        
        return triangle
```

## 404. Sum of Left Leaves
### DFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode], isLeftChild = False) -> int:
        if not root:
            return 0

        if not root.left and not root.right and isLeftChild:
            return root.val

        return self.sumOfLeftLeaves(root.left, True) + self.sumOfLeftLeaves(root.right, False)
```

### BFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

from collections import deque

class Solution:
    def sumOfLeftLeaves(self, root: Optional[TreeNode]) -> int:
        def isLeaf(node):
            return not node.left and not node.right
        
        queue = deque([[root, None]])
        sum = 0
        
        while queue:
            node, parent = queue.popleft()
            
            if isLeaf(node) and parent and node is parent.left:
                sum += node.val
                
            if node.left:
                queue.append([node.left, node])
            if node.right:
                queue.append([node.right, node])
                
        return sum
```

## 645. Set Mismatch
```python
class Solution:
    def findErrorNums(self, nums: List[int]) -> List[int]:
        seen = [0] * len(nums)
        
        for num in nums:
            seen[num - 1] += 1
                
        repeat = -1
        missing = -1
        
        for i in range(len(seen)):
            if seen[i] == 0:
                missing = i + 1
            elif seen[i] == 2:
                repeat = i + 1
        
        return [repeat, missing]
```

## 1464. Maximum Product of Two Elements in an Array
```python
class Solution:
    def maxProduct(self, nums: List[int]) -> int:
        max1 = 0
        max2 = 0
        
        for num in nums:
            if max1 < num:
                max2 = max1
                max1 = num
            elif max2 < num:
                max2 = num
        
        return (max1 - 1) * (max2 - 1)
```

## 237. Delete Node in a Linked List
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def deleteNode(self, node):
        """
        :type node: ListNode
        :rtype: void Do not return anything, modify node in-place instead.
        """
        node.val = node.next.val
        node.next = node.next.next
```

## 1528. Shuffle String
```python
class Solution:
    def restoreString(self, s: str, indices: List[int]) -> str:
        result = [0] * len(indices)
        
        for i in range(0, len(indices)):
            result[indices[i]] = s[i]
        
        return ''.join(result)
```

## 35. Search Insert Position
```python
class Solution:
    def searchInsert(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums)
        
        while left < right:
            mid = (right - left) // 2 + left
            
            if nums[mid] < target:
                left = mid + 1
            else:
                right = mid
                
        return left
```

## 167. Two Sum II - Input array is sorted
```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0
        right = len(numbers) - 1
        
        while left < right:
            sum = numbers[left] + numbers[right]
            
            if sum == target:
                return [left + 1, right + 1]
            elif sum < target:
                left += 1
            else:
                right -= 1
        
        return [-1, -1]
```

## 110. Balanced Binary Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isBalanced(self, root: Optional[TreeNode]) -> bool:
        def height(root):
            if not root:
                return 0
            
            leftHeight = height(root.left)
            rightHeight = height(root.right)
            
            diff = abs(leftHeight - rightHeight)
            
            if diff > 1:
                return math.inf
            
            return 1 + max(leftHeight, rightHeight)
        
        return height(root) != math.inf
```

## 852. Peak Index in a Mountain Array
```python
class Solution:
    def peakIndexInMountainArray(self, arr: List[int]) -> int:
        left = 1
        right = len(arr) - 2
        
        while left < right:
            mid = (right - left) // 2 + left
            
            if arr[mid] < arr[mid + 1]:
                left = mid + 1
            else:
                right = mid
            
        return left
```

## 496. Next Greater Element I
```python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        map = {}
        stack = []
        
        for num in nums2:
            map[num] = -1
            
            while stack and stack[-1] < num:
                map[stack.pop()] = num
                
            stack.append(num)
        
        result = []
        
        for num in nums1:
            result.append(map[num])
        
        return result
```

## 572. Subtree of Another Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSubtree(self, root: Optional[TreeNode], subRoot: Optional[TreeNode]) -> bool:
        def isEqual(root1, root2):
            if not root1 and not root2:
                return True
            if not root1 or not root2:
                return False
            
            if root1.val != root2.val:
                return False
            
            left = isEqual(root1.left, root2.left)
            right = isEqual(root1.right, root2.right)
            return left and right
        
        def _isSubtree(root):
            if not root:
                return False
            
            if root.merkle == subRoot.merkle and isEqual(root, subRoot):
                return True
            
            left = _isSubtree(root.left)
            right = _isSubtree(root.right)
            return left or right
        
        self.toMerkle(root)
        self.toMerkle(subRoot)
        
        return _isSubtree(root)
        
    def toMerkle(self, root):
        if not root:
            return 0
        
        left = self.toMerkle(root.left)
        right = self.toMerkle(root.right)
        
        root.merkle = hash(root.val + left + right)
        return root.merkle
```

## 338. Counting Bits
```python
class Solution:
    def countBits(self, n: int) -> List[int]:
        def countOnes(n):
            count = 0
            
            while n:
                n &= n - 1
                count += 1
            
            return count
        
        result = []
        
        for i in range(n + 1):
            result.append(countOnes(i))
            
        return result
```

## 204. Count Primes
```python
class Solution:
    def countPrimes(self, n: int) -> int:
        if n <= 1:
            return 0
        
        sieve = [True] * n
        sieve[0] = False
        sieve[1] = False
        
        count = 0
        for num in range(2, n):
            if not sieve[num]:
                continue
                
            count += 1
            multiplier = 2

            while num * multiplier < n:
                sieve[num * multiplier] = False
                multiplier += 1
                
        return count
```

## 172. Factorial Trailing Zeroes
```python
class Solution:
    def trailingZeroes(self, n: int) -> int:
        numOfFives = 0
        
        while n:
            n //= 5
            numOfFives += n
        
        return numOfFives
```

## 1331. Rank Transform of an Array
```python
class Solution:
    def arrayRankTransform(self, arr: List[int]) -> List[int]:
        sortedArr = sorted(arr)
        
        rank = 1
        rankMap = {}
        for num in sortedArr:
            if num in rankMap:
                continue
                
            rankMap[num] = rank
            rank += 1
            
        result = []
        
        for num in arr:
            result.append(rankMap[num])
        
        return result
```

## 270. Closest Binary Search Tree Value
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def closestValue(self, root: TreeNode, target: float) -> int:
        closest = root.val
        
        while root:
            if abs(target - closest) > abs(target - root.val):
                closest = root.val
            
            if root.val == target:
                return root.val
            elif root.val < target:
                root = root.right
            else:
                root = root.left
            
        return closest
```

## 20. Valid Parentheses
```python
class Solution:
    def isValid(self, s: str) -> bool:
        map = { "(": ")", "{": "}", "[": "]" }
        
        stack = []
        
        for char in s:
            if char in map:
                stack.append(char)
            else:
                if len(stack) == 0:
                    return False
                
                top = stack.pop()
                
                if map[top] != char:
                    return False
        
        return len(stack) == 0
```

## 605. Can Place Flowers
```python
class Solution:
    def canPlaceFlowers(self, flowerbed: List[int], n: int) -> bool:
        count = 0
        
        for i in range(len(flowerbed)):
            if flowerbed[i]:
                continue
                
            left = flowerbed[i - 1] if i > 0 else 0
            right = flowerbed[i + 1] if i < len(flowerbed) - 1 else 0
            
            if left == 0 and right == 0:
                flowerbed[i] = 1
                count += 1
                
        return count >= n
```

## 242. Valid Anagram
```python
from collections import Counter

class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        return Counter(s) == Counter(t)
```

## 157. Read N Characters Given Read4
```python
"""
The read4 API is already defined for you.

    @param buf4, a list of characters
    @return an integer
    def read4(buf4):

# Below is an example of how the read4 API can be called.
file = File("abcdefghijk") # File is "abcdefghijk", initially file pointer (fp) points to 'a'
buf4 = [' '] * 4 # Create buffer with enough space to store characters
read4(buf4) # read4 returns 4. Now buf = ['a','b','c','d'], fp points to 'e'
read4(buf4) # read4 returns 4. Now buf = ['e','f','g','h'], fp points to 'i'
read4(buf4) # read4 returns 3. Now buf = ['i','j','k',...], fp points to end of file
"""

class Solution:
    def read(self, buf, n):
        """
        :type buf: Destination buffer (List[str])
        :type n: Number of characters to read (int)
        :rtype: The number of actual characters read (int)
        """
        buf4 = [''] * 4
        writeIndex = 0
        
        while True:
            charsRead = read4(buf4)
            
            if not charsRead:
                break
            
            end = min(charsRead, n)
            for i in range(end):
                buf[writeIndex] = buf4[i]
                writeIndex += 1
                n -= 1
                
        return writeIndex
```

## 766. Toeplitz Matrix
```python
class Solution:
    def isToeplitzMatrix(self, matrix: List[List[int]]) -> bool:
        m = len(matrix)
        n = len(matrix[0])
        
        for row in range(m - 1):
            prev = matrix[row][:-1]
            curr = matrix[row + 1][1:]
            
            if prev != curr:
                return False
        
        return True
```

## 252. Meeting Rooms
```python
class Solution:
    def canAttendMeetings(self, intervals: List[List[int]]) -> bool:
        if not intervals:
            return True
        
        sortedIntervals = sorted(intervals, key = lambda m : m[0])
        
        end = sortedIntervals[0][1]
        
        for currStart, currEnd in sortedIntervals[1:]:
            if currStart < end:
                return False
            
            end = max(currEnd, end)
            
        return True
```

## 844. Backspace String Compare
```python
class Solution:
    def backspaceCompare(self, s: str, t: str) -> bool:
        sIndex = len(s) - 1
        sBackspaceCount = 0
        
        tIndex = len(t) - 1
        tBackspaceCount = 0
        
        while sIndex >= 0 or tIndex >= 0:
            if sIndex >= 0 and s[sIndex] == '#':
                sIndex -= 1
                sBackspaceCount += 1
                continue
            
            if tIndex >= 0 and t[tIndex] == '#':
                tIndex -= 1
                tBackspaceCount += 1
                continue
            
            if sIndex >= 0 and sBackspaceCount:
                sIndex -= 1
                sBackspaceCount -= 1
                continue
                
            if tIndex >= 0 and tBackspaceCount:
                tIndex -= 1
                tBackspaceCount -= 1
                continue
            
            if sIndex >= 0 and tIndex >= 0 and s[sIndex] == t[tIndex]:
                sIndex -= 1
                tIndex -= 1
                continue
                
            return False
        
        return sIndex < 0 and tIndex < 0
```

## 674. Longest Continuous Increasing Subsequence
```python
class Solution:
    def findLengthOfLCIS(self, nums: List[int]) -> int:
        maxLen = 1
        
        left = 0
        for right in range(1, len(nums)):
            if nums[right - 1] >= nums[right]:
                left = right
                
            maxLen = max(maxLen, right - left + 1)
            
        return maxLen
```

## 346. Moving Average from Data Stream
```python
class MovingAverage:

    def __init__(self, size: int):
        """
        Initialize your data structure here.
        """
        self.queue = deque()
        self.sum = 0
        self.maxSize = size

    def next(self, val: int) -> float:
        self.queue.append(val)
        self.sum += val
        
        while len(self.queue) > self.maxSize:
            oldVal = self.queue.popleft()
            self.sum -= oldVal
            
        return self.sum / len(self.queue)

# Your MovingAverage object will be instantiated and called as such:
# obj = MovingAverage(size)
# param_1 = obj.next(val)
```

## 13. Roman to Integer
```python
class Solution:
    def romanToInt(self, s: str) -> int:
        dict = { 'I': 1, 'V': 5, 'X': 10, 'L': 50, 'C': 100, 'D': 500, 'M': 1000 }
        
        result = 0
        
        i = 0
        while i < len(s):
            if i < len(s) - 1 and dict[s[i + 1]] > dict[s[i]]:
                result += dict[s[i + 1]] - dict[s[i]]
                i += 2
                continue
                
            result += dict[s[i]]
            i += 1
            
        return result
```

## 203. Remove Linked List Elements
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def removeElements(self, head: ListNode, val: int) -> ListNode:
        dummy = ListNode()
        curr = dummy
        curr.next = head
        
        while head:
            if head.val == val:
                curr.next = head.next
            else:
                curr = curr.next
                
            head = head.next
            
        return dummy.next
```

## 14. Longest Common Prefix
```python
class Solution:
    def longestCommonPrefix(self, strs: List[str]) -> str:
        if len(strs) == 0:
            return ""
        
        prefix = []
        
        for i in range(len(strs[0])):
            candidate = strs[0][i]
            
            for str in strs[1:]:
                if i >= len(str) or str[i] != candidate:
                    return ''.join(prefix)
                
            prefix.append(candidate)
                
        return ''.join(prefix)
```

## 884. Uncommon Words from Two Sentences
```python
from collections import Counter

class Solution:
    def uncommonFromSentences(self, s1: str, s2: str) -> List[str]:
        words1 = s1.split(" ")
        words2 = s2.split(" ")
        
        counter = Counter(words1 + words2)
        
        result = []        
        
        for key in counter:
            if counter[key] == 1:
                result.append(key)
                
        return result
```

## 191. Number of 1 Bits
```python
class Solution:
    def hammingWeight(self, n: int) -> int:
        count = 0
        
        while n:
            count += 1
            n &= (n - 1)
        
        return count
```

## 26. Remove Duplicates from Sorted Array
```python
class Solution:
    def removeDuplicates(self, nums: List[int]) -> int:
        i = 0
        for j in range(1, len(nums)):
            if nums[i] != nums[j]:
                i += 1
                nums[i] = nums[j]
                
        return i + 1
```

## 303. Range Sum Query - Immutable
```python
class NumArray:

    def __init__(self, nums: List[int]):
        self.prefixSums = [0]
        for num in nums:
            self.prefixSums.append(num + self.prefixSums[-1])
            
    def sumRange(self, left: int, right: int) -> int:
        return self.prefixSums[right + 1] - self.prefixSums[left]


# Your NumArray object will be instantiated and called as such:
# obj = NumArray(nums)
# param_1 = obj.sumRange(left,right)
```

## 94. Binary Tree Inorder Traversal
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        def inorder(root):
            if root is None:
                return
            
            inorder(root.left)
            result.append(root.val)
            inorder(root.right)
        
        result = []
        inorder(root)
        return result
```

## 219. Contains Duplicate II
```python
class Solution:
    def containsNearbyDuplicate(self, nums: List[int], k: int) -> bool:
        map = {}
        
        i = 0
        for j in range(len(nums)):
            map[nums[j]] = 1 + map.setdefault(nums[j], 0)
            
            if map[nums[j]] >= 2:
                return True
            
            if j - i == k:
                map[nums[i]] -= 1
                i += 1
                
        return False
```

## 69. Sqrt(x)
```python
class Solution:
    def mySqrt(self, x: int) -> int:
        left = 0
        right = x // 2 + 1
        
        while left < right:
            mid = (right - left + 1) // 2 + left            
            square = mid ** 2
            
            if square > x:
                right = mid - 1
            else:
                left = mid
                
        return left
```

## 345. Reverse Vowels of a String
```python
class Solution:
    def reverseVowels(self, s: str) -> str:
        def isVowel(char):
            vowels = {'a', 'e', 'i', 'o', 'u'}
            return char.lower() in vowels
        
        result = list(s)
        
        left = 0
        right = len(result) - 1
        
        while (left < right):
            if not isVowel(result[left]):
                left += 1
                continue
                
            if not isVowel(result[right]):
                right -= 1
                continue
                
            result[left], result[right] = result[right], result[left]
            left += 1
            right -= 1
        
        return ''.join(result)
```

## 66. Plus One
```python
class Solution:
    def plusOne(self, digits: List[int]) -> List[int]:
        carry = 1
        
        for i in range(len(digits) - 1, -1, -1):
            sum = carry + digits[i]
            digits[i] = sum % 10
            carry = sum // 10
            
        if carry > 0:
            digits = [carry] + digits
            
        return digits
```

## 145. Binary Tree Postorder Traversal
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def postorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        def postorder(root):
            if root is None:
                return
            
            postorder(root.left)
            postorder(root.right)
            result.append(root.val)
            
        result = []
        postorder(root)
        return result
```

## 136. Single Number
```python
class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        x = 0
        
        for num in nums:
            x ^= num
        
        return x
```

## 392. Is Subsequence
```python
class Solution:
    def isSubsequence(self, s: str, t: str) -> bool:
        i = 0
        j = 0
        
        while i < len(s) and j < len(t):
            if s[i] == t[j]:
                i += 1
                j += 1
                continue
            
            j += 1
        
        return i == len(s)
```

## 112. Path Sum
### DFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        def pathSum(root, currSum):
            if root is None:
                return False
            
            currSum += root.val
            
            if root.left is None and root.right is None:
                return currSum == targetSum
            
            return pathSum(root.left, currSum) or pathSum(root.right, currSum)
        
        return pathSum(root, 0)
```

### BFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def hasPathSum(self, root: Optional[TreeNode], targetSum: int) -> bool:
        if not root:
            return False
        
        queue = deque([[root, 0]])
        while queue:
            node, sum = queue.popleft()
            
            if not node.left and not node.right and sum + node.val == targetSum:
                return True
            
            if node.left:
                queue.append([node.left, node.val + sum])
            if node.right:
                queue.append([node.right, node.val + sum])
        
        return False
```

## 101. Symmetric Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        def check(root1, root2):
            if root1 is None and root2 is None:
                return True
            if root1 is None or root2 is None:
                return False
            
            if root1.val != root2.val:
                return False
            
            return check(root1.left, root2.right) and check(root1.right, root2.left)
        
        return check(root, root)
```

## 83. Remove Duplicates from Sorted List
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
        dummy = ListNode(None)
        dummy.next = head
        curr = dummy
        
        while head:
            if curr.val != head.val:
                curr.next = head
                curr = curr.next
                
            head = head.next
            
        curr.next = head
        
        return dummy.next
```

## 1662. Check If Two String Arrays are Equivalent
```python
class Solution:
    def arrayStringsAreEqual(self, word1: List[str], word2: List[str]) -> bool:
        arr1Index = 0
        arr2Index = 0
        
        str1Index = 0
        str2Index = 0
        
        while arr1Index < len(word1) and arr2Index < len(word2):
            if str1Index >= len(word1[arr1Index]):
                str1Index = 0
                arr1Index += 1
                continue
                
            if str2Index >= len(word2[arr2Index]):
                str2Index = 0
                arr2Index += 1
                continue
                
            if word1[arr1Index][str1Index] == word2[arr2Index][str2Index]:
                str1Index += 1
                str2Index += 1
                
            return False
                
        if arr1Index < len(word1) and str1Index >= len(word1[arr1Index]):
            str1Index = 0
            arr1Index += 1
            
        if arr2Index < len(word2) and str2Index >= len(word2[arr2Index]):
            str2Index = 0
            arr2Index += 1
        
        return arr1Index == len(word1) and arr2Index == len(word2)
```

## 232. Implement Queue using Stacks
```python
class MyQueue:

    def __init__(self):
        """
        Initialize your data structure here.
        """
        self.stack1 = []
        self.stack2 = []
        
    def push(self, x: int) -> None:
        """
        Push element x to the back of queue.
        """
        self.stack1.append(x)

    def pop(self) -> int:
        """
        Removes the element from in front of queue and returns that element.
        """
        self.balance()
        return self.stack2.pop()

    def peek(self) -> int:
        """
        Get the front element.
        """
        self.balance()
        return self.stack2[-1]

    def empty(self) -> bool:
        """
        Returns whether the queue is empty.
        """
        return not self.stack1 and not self.stack2
    
    def balance(self):
        if not self.stack2:
            while(self.stack1):
                self.stack2.append(self.stack1.pop())

# Your MyQueue object will be instantiated and called as such:
# obj = MyQueue()
# obj.push(x)
# param_2 = obj.pop()
# param_3 = obj.peek()
# param_4 = obj.empty()
```

## 1304. Find N Unique Integers Sum up to Zero
```python
class Solution:
    def sumZero(self, n: int) -> List[int]:
        result = []
        
        if n % 2 == 1:
            result.append(0)
            n -= 1
            
        for num in range(1, n, 2):
            result.append(num)
            result.append(-num)
            
        return result
```

## 541. Reverse String II
```python
class Solution:
    def reverseStr(self, s: str, k: int) -> str:
        def reverse(arr, left, right):
            while left < right:
                arr[left], arr[right] = arr[right], arr[left]
                left += 1
                right -=1
        
        result = list(s)
        
        for i in range(0, len(s), 2 * k):
            reverse(result, i, min(i + k - 1, len(result) - 1))
        
        return ''.join(result)
```

## 1748. Sum of Unique Elements
```python
from collections import Counter

class Solution:
    def sumOfUnique(self, nums: List[int]) -> int:
        counter = Counter(nums)
        sum = 0
        
        for num in nums:
            sum += num if counter[num] == 1 else 0
            
        return sum
```

## 485. Max Consecutive Ones
```python
class Solution:
    def findMaxConsecutiveOnes(self, nums: List[int]) -> int:
        maxCount = 0
        currCount = 0
        
        for num in nums:
            if num == 1:
                currCount += 1
            else:
                maxCount = max(maxCount, currCount)
                currCount = 0
        
        maxCount = max(maxCount, currCount)
        
        return maxCount
```

## 1137. N-th Tribonacci Number
```python
class Solution:
    def tribonacci(self, n: int) -> int:
        def t(n):
            if n == 0:
                return 0
            if n <= 2:
                return 1
            
            if n in memo:
                return memo[n]
            
            memo[n] = t(n - 3) + t(n - 2) + t(n - 1)
            return memo[n]
        
        memo = {}
        return t(n)
```

## 1920. Build Array from Permutation
```python
class Solution:
    def buildArray(self, nums: List[int]) -> List[int]:
        result = [0] * len(nums)
        
        for i in range(len(result)):
            result[i] = nums[nums[i]]
        
        return result
```

## 1773. Count Items Matching a Rule
```python
class Solution:
    def countMatches(self, items: List[List[str]], ruleKey: str, ruleValue: str) -> int:
        sum = 0
        
        for type, color, name in items:
            if ruleKey == "type":
                sum += type == ruleValue
            if ruleKey == "color":
                sum += color == ruleValue
            if ruleKey == "name":
                sum += name == ruleValue
        
        return sum
```

## 163. Missing Ranges
```python
class Solution:
    def findMissingRanges(self, nums: List[int], lower: int, upper: int) -> List[str]:
        def strFromRange(start, end):
            if start == end:
                return f'{start}'
            
            return f'{start}->{end}'
        
        result = []
        
        if not nums:
            result.append(strFromRange(lower, upper))
            return result
            
        if lower < nums[0]:
            result.append(strFromRange(lower, nums[0] - 1))
        
        for i in range(0, len(nums) - 1):
            start = nums[i] + 1
            end = nums[i + 1] - 1
            
            if start > end:
                continue
            
            result.append(strFromRange(start, end))
        
        if nums[-1] < upper:
            result.append(strFromRange(nums[-1] + 1, upper))
            
        return result
```

## 94. Binary Tree Inorder Traversal
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def inorderTraversal(self, root: Optional[TreeNode]) -> List[int]:
        def inorder(root):
            if root is None:
                return
            
            inorder(root.left)
            result.append(root.val)
            inorder(root.right)
            
        result = []
        inorder(root)
        return result
```

## 7. Reverse Integer
```python
class Solution:
    def reverse(self, x: int) -> int:
        sign = -1 if x < 0 else 1
        
        reversedNum = 0
        x = abs(x)
        
        upper = (2 ** 31 - 1) // 10
        
        while x:
            digit = x % 10
            
            if reversedNum > upper or reversedNum == upper and digit > 7:
                return 0
            
            reversedNum *= 10
            reversedNum += digit
            
            x //= 10
            
        return sign * reversedNum
```

## 228. Summary Ranges
```python
class Solution:
    def summaryRanges(self, nums: List[int]) -> List[str]:
        def strForRange(start, end):
            if start == end:
                return f'{start}'
            
            return f'{start}->{end}'
        
        if not nums:
            return []
        
        result = []
        
        start = 0
        for end in range(1, len(nums) + 1):
            if end < len(nums) and nums[end - 1] == nums[end] - 1:
                continue
                
            result.append(strForRange(nums[start], nums[end - 1]))
            start = end
            
        return result
```

## 160. Intersection of Two Linked Lists
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        a = headA
        b = headB
        
        while a != b:
            a = a.next if a else headB
            b = b.next if b else headA

        return a
```

## 703. Kth Largest Element in a Stream
```python
class KthLargest:

    def __init__(self, k: int, nums: List[int]):
        self.k = k
        self.heap = nums
        heapq.heapify(self.heap)

    def add(self, val: int) -> int:
        heapq.heappush(self.heap, val)
        
        while len(self.heap) > self.k:
            heapq.heappop(self.heap)
            
        return self.heap[0]
        


# Your KthLargest object will be instantiated and called as such:
# obj = KthLargest(k, nums)
# param_1 = obj.add(val)
```

## 1275. Find Winner on a Tic Tac Toe Game
```python
class Solution:
    def tictactoe(self, moves: List[List[int]]) -> str:
        size = 3
        
        rows = [0] * size
        cols = [0] * size
        diag = 0
        antiDiag = 0
        
        aTurn = True
        for row, col in moves:
            val = 1 if aTurn else -1
            score = size * val
            
            rows[row] += val
            cols[col] += val
                        
            if row - col == 0:
                diag += val

            if row + col == size - 1:
                antiDiag += val

            if rows[row] == score or cols[col] == score or diag == score or antiDiag == score:
                return 'A' if aTurn else 'B'
            
            aTurn = not aTurn
            
        if len(moves) == size ** 2:
            return 'Draw'
        
        return 'Pending'
```

## 234. Palindrome Linked List
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def isPalindrome(self, head: Optional[ListNode]) -> bool:
        def getMiddle(head):
            fast = head
            slow = head
            
            while fast.next and fast.next.next:
                fast = fast.next.next
                slow = slow.next
                
            return slow
        
        def reverse(head):
            prev = None
            next = None
            
            while head:
                next = head.next
                head.next = prev
                prev = head
                head = next
            
            return prev
        
        def isEqual(l1, l2):
            while l1 and l2:
                if l1.val != l2.val:
                    return False
                l1 = l1.next
                l2 = l2.next
            return True
        
        if not head.next:
            return True
        
        # get mid point
        middleNode = getMiddle(head)
        secondHalfHead = middleNode.next
        middleNode.next = None
        
        # reverse
        reversedHead = reverse(secondHalfHead)
        
        # check if equal
        return isEqual(head, reversedHead)
```

## 235. Lowest Common Ancestor of a Binary Search Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        while root:
            if root.val < p.val and root.val < q.val:
                root = root.right  
            elif root.val > p.val and root.val > q.val:
                root = root.left
            else:
                return root
```

## 290. Word Pattern
```python
class Solution:
    def wordPattern(self, pattern: str, s: str) -> bool:
        words = s.split(' ')
        
        if len(pattern) != len(words):
            return False
        
        dictP = {}
        dictW = {}
        
        for i in range(len(pattern)):
            p = pattern[i]
            w = words[i]
            
            if p not in dictP and w not in dictW:
                dictP[p] = w
                dictW[w] = p
                continue
                
            if p not in dictP or w not in dictW:
                return False
            
            if dictP[p] != w or dictW[w] != p:
                return False
            
        return True
```

## 653. Two Sum IV - Input is a BST
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findTarget(self, root: Optional[TreeNode], k: int) -> bool:
        def inorder(root):
            if root is None:
                return
            
            inorder(root.left)
            sorted.append(root.val)
            inorder(root.right)
        
        sorted = []
        inorder(root)
        
        left = 0
        right = len(sorted) - 1
        
        while left < right:
            sum = sorted[left] + sorted[right]
            
            if sum == k:
                return True
            elif sum < k:
                left += 1
            else:
                right -= 1
                
        return False
```

## 1636. Sort Array by Increasing Frequency
```python
from collections import Counter

class Solution:
    def frequencySort(self, nums: List[int]) -> List[int]:
        counter = Counter(nums)
        return sorted(nums, key = lambda num : (counter[num], -num))
```

## 1122. Relative Sort Array
```python
class Solution:
    def relativeSortArray(self, arr1: List[int], arr2: List[int]) -> List[int]:
        def getKey(num):
            if num in dict:
                return (dict[num], num)
            return (math.inf, num)
        
        dict = {}
        for i in range(len(arr2)):
            dict[arr2[i]] = i
            
        return sorted(arr1, key = getKey)
```

## 905. Sort Array By Parity
```python
class Solution:
    def sortArrayByParity(self, nums: List[int]) -> List[int]:
        i = 0
        for j in range(len(nums)):
            if nums[j] % 2 == 0:
                nums[i], nums[j] = nums[j], nums[i]
                i += 1
                
        return nums
```

## 215. Kth Largest Element in an Array
### Heap
```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        heap = []
        
        for num in nums:
            heapq.heappush(heap, num)
            
            if len(heap) > k:
                heapq.heappop(heap)
                
        return heap[0]
```

### Quickselect
```python
class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        index = self.quickSelect(nums, len(nums) - k)
        return nums[index]
    
    def quickSelect(self, nums, k):
        def partition(nums, left, right):
            i = left - 1
            for j in range(left, right):
                if nums[j] <= nums[right]:
                    i += 1
                    nums[i], nums[j] = nums[j], nums[i]
            
            i += 1
            nums[i], nums[right] = nums[right], nums[i]
            return i
        
        left = 0
        right = len(nums) - 1
        
        while left <= right:
            randomIndex = random.randint(left, right)
            nums[right], nums[randomIndex] = nums[randomIndex], nums[right]
            
            partitionIndex = partition(nums, left, right)
            if partitionIndex == k:
                return partitionIndex
            elif partitionIndex < k:
                left = partitionIndex + 1
            else:
                right = partitionIndex - 1
                
        return -1
```

## 438. Find All Anagrams in a String
```python
class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        def indexForChar(char):
            return ord(char) - ord('a')
        
        result = []
        
        sCounter = [0] * 26
        pCounter = [0] * 26
        for char in p:
            pCounter[indexForChar(char)] += 1
            
        left = 0
        for right in range(len(s)):
            sCounter[indexForChar(s[right])] += 1
            
            while right - left + 1 > len(p):
                sCounter[indexForChar(s[left])] -= 1
                left += 1
                
            if sCounter == pCounter:
                result.append(left)
        
        return result
```

## 1570. Dot Product of Two Sparse Vectors
https://leetcode.com/discuss/interview-question/124823/dot-product-of-sparse-vector
```python
class SparseVector:
    def __init__(self, nums: List[int]):
        self.vals = {}
        
        for i in range(len(nums)):
            if nums[i] == 0:
                continue
            
            self.vals[i] = nums[i]
            
    # Return the dotProduct of two sparse vectors
    def dotProduct(self, vec: 'SparseVector') -> int:
        vecShort = self if len(self.vals) < len(vec.vals) else vec
        vecLong = vec if len(self.vals) < len(vec.vals) else self
        
        sum = 0
        
        for i in vecShort.vals:
            if i in vecLong.vals:
                sum += vecShort.vals[i] * vecLong.vals[i]
        
        return sum

# Your SparseVector object will be instantiated and called as such:
# v1 = SparseVector(nums1)
# v2 = SparseVector(nums2)
# ans = v1.dotProduct(v2)
```

## 199. Binary Tree Right Side View
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        result = []
        queue = deque([root])
        
        while queue:
            size = len(queue)
            
            for i in range(size):
                node = queue.popleft()
                
                if i == 0:
                    result.append(node.val)
                
                if node.right:
                    queue.append(node.right)
                if node.left:
                    queue.append(node.left)
            
        return result
```

## 304. Range Sum Query 2D - Immutable
```python
class NumMatrix:
    def __init__(self, matrix: List[List[int]]):
        rowLen = len(matrix)
        colLen = len(matrix[0])

        self.prefix = [[0] * (colLen + 1) for i in range(rowLen + 1)]

        for row in range(1, rowLen + 1):
            for col in range(1, colLen + 1):
                self.prefix[row][col] = (matrix[row - 1][col - 1] +  
                                         self.prefix[row - 1][col] +
                                         self.prefix[row][col - 1] -
                                         self.prefix[row - 1][col - 1])
                
    def sumRegion(self, row1: int, col1: int, row2: int, col2: int) -> int:
        return (self.prefix[row2 + 1][col2 + 1] + 
                self.prefix[row1][col1] - 
                self.prefix[row2 + 1][col1] - 
                self.prefix[row1][col2 + 1])


# Your NumMatrix object will be instantiated and called as such:
# obj = NumMatrix(matrix)
# param_1 = obj.sumRegion(row1,col1,row2,col2)
```

## 91. Decode Ways
### Top Down DP
```python
class Solution:
    def numDecodings(self, s: str) -> int:
        def _numDecodings(startIndex):
            if startIndex in memo:
                return memo[startIndex]
            
            if startIndex >= len(s):
                return 1
            
            count = 0
            
            num = 0
            for currIndex in range(startIndex, len(s)):
                num *= 10
                num += int(s[currIndex])
                
                if num == 0 or num > 26:
                    break
                    
                count += _numDecodings(currIndex + 1)
                
            memo[startIndex] = count
            return count
              
        memo = {}
        return _numDecodings(0)
```

### Bottom Up DP
```python
class Solution:
    def numDecodings(self, s: str) -> int:
        dpStepBack2 = 1
        dpStepBack1 = 0 if int(s[0]) == 0 else 1
        
        for i in range(2, len(s) + 1):
            dpStepBack0 = 0
            
            oneDigit = int(s[i - 1])
            if oneDigit > 0:
                dpStepBack0 += dpStepBack1
            
            twoDigit = int(s[i - 2: i])
            if 10 <= twoDigit <= 26:
                dpStepBack0 += dpStepBack2
            
            dpStepBack2 = dpStepBack1
            dpStepBack1 = dpStepBack0
            
        return dpStepBack1
```

## 133. Clone Graph
### DFS
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val = 0, neighbors = None):
        self.val = val
        self.neighbors = neighbors if neighbors is not None else []
"""

class Solution:
    def cloneGraph(self, node: 'Node') -> 'Node':
        def dfs(node):
            if node.val in copyDict:
                return copyDict[node.val]
            
            copyNode = Node(node.val)
            copyDict[node.val] = copyNode

            for neighbor in node.neighbors:
                copyNeighbor = dfs(neighbor)
                copyNode.neighbors.append(copyNeighbor)
                
            return copyNode
        
        if not node:
            return node
        
        copyDict = {}
        dfs(node)
        return copyDict[node.val]
```

## 986. Interval List Intersections
```python
class Solution:
    def intervalIntersection(self, firstList: List[List[int]], secondList: List[List[int]]) -> List[List[int]]:
        intersections = []
        
        i = 0
        j = 0
        
        while i < len(firstList) and j < len(secondList):
            firstStart, firstEnd = firstList[i]
            secondStart, secondEnd = secondList[j]
            
            candidateStart = max(firstStart, secondStart)
            candidateEnd = min(firstEnd, secondEnd)
            
            if candidateStart <= candidateEnd:
                intersections.append([candidateStart, candidateEnd])
                
            if firstEnd <= secondEnd:
                i += 1
            else:
                j += 1
        
        return intersections
        
```

## 340. Longest Substring with At Most K Distinct Characters
```python
from collections import Counter

class Solution:
    def lengthOfLongestSubstringKDistinct(self, s: str, k: int) -> int:
        longest = 0
        
        counter = Counter()
        left = 0
        for right in range(len(s)):
            counter[s[right]] += 1
            
            while left <= right and len(counter) > k:
                counter[s[left]] -= 1
                
                if counter[s[left]] == 0:
                    del counter[s[left]]
                    
                left += 1
                
            longest = max(longest, right - left + 1)
            
        return longest
```

## 17. Letter Combinations of a Phone Number
### Backtracking
```python
class Solution:
    def letterCombinations(self, digits: str) -> List[str]:
        def _letterCombinations(i):
            if i >= len(digits):
                result.append(''.join(combo))
                return
            
            for char in dict[digits[i]]:
                combo.append(char)
                _letterCombinations(i + 1)
                combo.pop()
                
        if len(digits) == 0:
            return []
                
        dict = {'2': 'abc', '3': 'def', '4': 'ghi', '5': 'jkl', 
                '6': 'mno', '7': 'pqrs', '8': 'tuv', '9': 'wxyz'}
        
        result = []
        combo = []
        
        _letterCombinations(0)
        
        return result
```

## 139. Word Break
### Top Down DP
```python
class Solution:
    def wordBreak(self, s: str, wordDict: List[str]) -> bool:
        def _wordBreak(startIndex):
            if startIndex in memo:
                return memo[startIndex]
            
            if startIndex >= len(s):
                return True
            
            for i in range(startIndex, min(startIndex + maxLen, len(s))):
                substr = s[startIndex : i + 1]
                if substr in dict and _wordBreak(i + 1):
                    memo[startIndex] = True
                    return True
                
            memo[startIndex] = False
            return False
        
        dict = set()
        maxLen = 0
        for word in wordDict:
            dict.add(word)
            maxLen = max(maxLen, len(word))
        
        memo = {}
        return _wordBreak(0)
```

## 200. Number of Islands
### BFS
```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        islands = 0
        
        for row in range(len(grid)):
            for col in range(len(grid[row])):
                if grid[row][col] == "1":
                    islands += 1
                    self.floodFill(grid, row, col)
        
        return islands
    
    def floodFill(self, grid, row, col):
        queue = deque([[row, col]])
        grid[row][col] = "0"
        
        dirs = [[1, 0], [0, 1], [-1, 0], [0, -1]]
        
        while queue:
            row, col = queue.popleft()
            
            for deltaRow, deltaCol in dirs:
                nextRow = row + deltaRow
                nextCol = col + deltaCol
                
                if nextRow < 0 or nextRow >= len(grid) or nextCol < 0 or nextCol >= len(grid[0]):
                    continue
                    
                if grid[nextRow][nextCol] == "1":
                    grid[nextRow][nextCol] = "0"
                    queue.append([nextRow, nextCol])         
```

### DFS
```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        islands = 0
        
        for row in range(len(grid)):
            for col in range(len(grid[row])):
                if grid[row][col] == "1":
                    islands += 1
                    self.floodFill(grid, row, col)
        
        return islands
    
    def floodFill(self, grid, row, col):
        def dfs(row, col):
            for deltaRow, deltaCol in dirs:
                nextRow = row + deltaRow
                nextCol = col + deltaCol

                if nextRow < 0 or nextRow >= len(grid) or nextCol < 0 or nextCol >= len(grid[0]):
                    continue

                if grid[nextRow][nextCol] == "1":
                    grid[nextRow][nextCol] = "0"
                    dfs(nextRow, nextCol)
                    
        grid[row][col] = "0"
        dirs = [[1, 0], [0, 1], [-1, 0], [0, -1]]
        dfs(row, col)
```

### Union Find
```python
class Solution:
    def numIslands(self, grid: List[List[str]]) -> int:
        rowLen = len(grid)
        colLen = len(grid[0])
        
        unionFind = UnionFind(rowLen * colLen)
        waterCount = 0
        
        dirs = [[1, 0], [0, 1]]
        
        for row in range(len(grid)):
            for col in range(len(grid[row])):
                if grid[row][col] == "1":
                    for deltaRow, deltaCol in dirs:
                        nextRow = row + deltaRow
                        nextCol = col + deltaCol

                        if nextRow >= len(grid) or nextCol >= len(grid[0]) or grid[nextRow][nextCol] == "0":
                            continue
                            
                        unionFind.union(row * colLen + col, nextRow * colLen + nextCol)
                else:
                    waterCount += 1
        
        return unionFind.numOfComponents - waterCount
    
class UnionFind:
    def __init__(self, n):
        self.size = []
        self.parent = []
        self.numOfComponents = n
        
        for i in range(0, n):
            self.size.append(1)
            self.parent.append(i)
            
    def union(self, a, b):
        parentA = self.find(a)
        parentB = self.find(b)
        
        if parentA == parentB:
            return
        
        if self.size[parentA] < self.size[parentB]:
            self.parent[parentA] = parentB
            self.size[parentB] += self.size[parentA]
        else:
            self.parent[parentB] = parentA
            self.size[parentA] += self.size[parentB]
        
        self.numOfComponents -= 1
    
    def find(self, a):
        root = a
        
        while self.parent[root] != root:
            root = self.parent[root]
            
        while a != root:
            next = self.parent[a]
            self.parent[a] = root
            a = next
            
        return root
```

## 785. Is Graph Bipartite?
```python
class Solution:
    def isBipartite(self, graph: List[List[int]]) -> bool:
        def dfs(node, color):
            colors[node] = color
            
            for neighbor in graph[node]:
                if colors[neighbor] == color:
                    return False
                
                if colors[neighbor] != None:
                    continue
                
                if not dfs(neighbor, color ^ 1):
                    return False
            
            return True
            
        n = len(graph)
        colors = [None] * n
        
        for node in range(len(graph)):
            if colors[node] == None:
                if not dfs(node, 0):
                    return False
                
        return True
```

## 325. Maximum Size Subarray Sum Equals k
```python
class Solution:
    def maxSubArrayLen(self, nums: List[int], k: int) -> int:
        dict = { 0: -1 }
        sum = 0
        longest = 0
        
        for i, num in enumerate(nums):
            sum += num
            
            candidate = sum - k
            
            if candidate in dict:
                longest = max(longest, i - dict[candidate])
            
            if sum not in dict:
                dict[sum] = i
            
        return longest
```

## 528. Random Pick with Weight
```python
class Solution:

    def __init__(self, w: List[int]):
        self.prefixSums = w
        
        for i in range(1, len(w)):
            self.prefixSums[i] = w[i] + self.prefixSums[i - 1]
        
    def pickIndex(self) -> int:
        randomIndex = randint(1, self.prefixSums[-1])
        
        left = 0
        right = len(self.prefixSums) - 1
        
        while left < right:
            mid = (right - left) // 2 + left
            
            if self.prefixSums[mid] < randomIndex:
                left = mid + 1
            else:
                right = mid
            
        return left
        
# Your Solution object will be instantiated and called as such:
# obj = Solution(w)
# param_1 = obj.pickIndex()
```

## 236. Lowest Common Ancestor of a Binary Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def lowestCommonAncestor(self, root: 'TreeNode', p: 'TreeNode', q: 'TreeNode') -> 'TreeNode':
        def _lowestCommonAncestor(node):
            if not node:
                return node
            
            if node is p or node is q:
                return node
            
            left = _lowestCommonAncestor(node.left)            
            right = _lowestCommonAncestor(node.right)
            
            if left and right:
                return node
            
            return left or right
        
        return _lowestCommonAncestor(root)
```

## 146. LRU Cache
```python
class LRUCache:

    def __init__(self, capacity: int):
        self.keyToNodeMap = {}
        self.linkedList = LinkedList()
        self.capacity = capacity
        self.size = 0

    def get(self, key: int) -> int:
        # if key doesn't exist in cache
        if key not in self.keyToNodeMap:
            return -1
        
        # node is now most recently used, move to front of list
        node = self.keyToNodeMap[key]
        self.linkedList.moveToHead(node)
        return node.value
    
    def put(self, key: int, value: int) -> None:
        # if key already exists, update the value
        # node is now most recently used, move to front of list
        if key in self.keyToNodeMap:
            node = self.keyToNodeMap[key]
            node.value = value
            self.linkedList.moveToHead(node)
            return
        
        # if at or over capacity evict tail
        if self.size >= self.capacity:
            # delete least recently used
            oldNode = self.linkedList.pop()
            oldKey = oldNode.key
            del self.keyToNodeMap[oldKey]
            self.size -= 1
        
        node = Node(key, value)
        self.keyToNodeMap[key] = node
        self.linkedList.insertAtHead(node)
        self.size += 1
        

class Node:

    def __init__(self, key, value):
        self.key = key
        self.value = value
        self.next = None
        self.prev = None

class LinkedList:
    
    def __init__(self):
        self.head = Node(None, None)
        self.tail = Node(None, None)
        
        self.head.next = self.tail
        self.tail.prev = self.head
        
    def insertAtHead(self, node):
        next = self.head.next
        
        next.prev = node
        node.next = next
        
        node.prev = self.head
        self.head.next = node
    
    def moveToHead(self, node):
        self.remove(node)
        self.insertAtHead(node)
    
    def pop(self) -> Node:
        if self.tail.prev is self.head:
            return None
        
        return self.remove(self.tail.prev)
    
    def remove(self, node):
        next = node.next
        prev = node.prev
        
        prev.next = next
        next.prev = prev
        
        node.prev = None
        node.next = None
        
        return node
        
        
# Your LRUCache object will be instantiated and called as such:
# obj = LRUCache(capacity)
# param_1 = obj.get(key)
# obj.put(key,value)
```

## 398. Random Pick Index
```python
class Solution:

    def __init__(self, nums: List[int]):
        self.nums = nums

    def pick(self, target: int) -> int:
        index = -1
        count = 0
        
        for i, num in enumerate(self.nums):
            if num == target:
                count += 1
                
                if random.randint(1, count) == 1:
                    index = i
                
            
        return index


# Your Solution object will be instantiated and called as such:
# obj = Solution(nums)
# param_1 = obj.pick(target)
```

## 71. Simplify Path
```python
class Solution:
    def simplifyPath(self, path: str) -> str:
        canonicalPath = []
        
        for section in path.split('/'):
            if section == '' or section == '.':
                continue
                
            if section == '..':
                if canonicalPath:
                    canonicalPath.pop()
                continue
                
            canonicalPath.append(section)
            
        return '/' + '/'.join(canonicalPath)
```

## 98. Validate Binary Search Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isValidBST(self, root: Optional[TreeNode]) -> bool:
        def _isValidBST(root, min, max):
            if not root:
                return True
            
            if min >= root.val or root.val >= max:
                return False
            
            return (_isValidBST(root.left, min, root.val) and 
                    _isValidBST(root.right, root.val, max))
        
        return _isValidBST(root, -math.inf, math.inf)
```

## 1150. Check If a Number Is Majority Element in a Sorted Array
```python
class Solution:
    def isMajorityElement(self, nums: List[int], target: int) -> bool:
        def binarySearch(nums, target): 
            left = 0
            right = len(nums)
            
            while left < right:
                mid = (right - left) // 2 + left
                
                if nums[mid] < target:
                    left = mid + 1
                else:
                    right = mid
                
            return left
        
        firstIndex = binarySearch(nums, target)
        if firstIndex >= len(nums) or nums[firstIndex] != target:
            return False
        
        lastIndex = binarySearch(nums, target + 1) - 1
        
        return lastIndex - firstIndex + 1 > len(nums) // 2
```

## 227. Basic Calculator II
```python
class Solution:
    def calculate(self, s: str) -> int:
        def hasHigherPrecendence(op1, op2):
            return getWeight(op1) >= getWeight(op2)

        def getWeight(op):
            if op in '+-':
                return 1
            if op in '*/':
                return 2
            
        def convertToPostfix(infix):
            postfix = []
            stack = []
            
            num = None
            
            for i, token in enumerate(infix):
                # Ignore Whitespace
                if token == ' ':
                    continue
                    
                # Handles case where operand is multiple digits
                if isOperand(token):
                    if num is None:
                        num = 0
                        
                    num *= 10
                    num += int(token)
                    continue
                    
                # Add operand to postfix before continuing
                if num is not None:
                    postfix.append(str(num))
                    num = None
                    
                if isOperator(token):
                    # if uniary '+' then skip
                    if token == '+' and i - 1 < 0 or infix[i - 1] == '(':
                        continue
            
                    # if uniary '-' convert to binary operator by inserting 0 on left side
                    if token == '-' and i - 1 < 0 or infix[i - 1] == '(':
                        postfix.append(str(0))
            
                    # remove higher precendence operators from stack first
                    while stack and stack[-1] != '(' and hasHigherPrecendence(stack[-1], token):
                        postfix.append(str(stack.pop()))

                    stack.append(str(token))

                if token == '(':
                    stack.append(str(token))

                # remove all operators up to the next open '(' in stack
                if token == ')':
                    while stack and stack[-1] != '(':
                        postfix.append(str(stack.pop()))

                    stack.pop()

            # Add final num of expression
            if num is not None:
                postfix.append(str(num))
                num = None

            # Add remaining operators to end
            while stack:
                postfix.append(stack.pop())
    
            return postfix
    
        def evaluatePostfix(postfix):
            evaluate = {
                '+': (lambda x1, x2 : x1 + x2),
                '-': (lambda x1, x2 : x1 - x2),
                '*': (lambda x1, x2 : x1 * x2),
                '/': (lambda x1, x2 : x1 // x2),
            }
            
            stack = []
            
            for token in postfix:
                if isOperand(token):
                    stack.append(token)
                    continue
                
                right = int(stack.pop())
                left = int(stack.pop())
                stack.append(evaluate[token](left, right))
            
            return stack.pop()
        
        def isOperand(token):
            return token not in '+-/*'
        
        def isOperator(token):
            return token in '+-/*'
        
        # convert to postfix expression
        postfix = convertToPostfix(s)
        print(postfix)
        
        # evaluate postfix expression
        return evaluatePostfix(postfix)
```

## 34. Find First and Last Position of Element in Sorted Array
```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        def binarySearch(nums, target):
            left = 0
            right = len(nums)
            
            while left < right:
                mid = (right - left) // 2 + left
                
                if nums[mid] < target:
                    left = mid + 1
                else:
                    right = mid
                    
            return left
        
        firstIndex = binarySearch(nums, target)
        if firstIndex >= len(nums) or nums[firstIndex] != target:
            return [-1, -1]
        
        lastIndex = binarySearch(nums, target + 1) - 1
            
        return [firstIndex, lastIndex]
```

## 339. Nested List Weight Sum
```python
# """
# This is the interface that allows for creating nested lists.
# You should not implement it, or speculate about its implementation
# """
#class NestedInteger:
#    def __init__(self, value=None):
#        """
#        If value is not specified, initializes an empty list.
#        Otherwise initializes a single integer equal to value.
#        """
#
#    def isInteger(self):
#        """
#        @return True if this NestedInteger holds a single integer, rather than a nested list.
#        :rtype bool
#        """
#
#    def add(self, elem):
#        """
#        Set this NestedInteger to hold a nested list and adds a nested integer elem to it.
#        :rtype void
#        """
#
#    def setInteger(self, value):
#        """
#        Set this NestedInteger to hold a single integer equal to value.
#        :rtype void
#        """
#
#    def getInteger(self):
#        """
#        @return the single integer that this NestedInteger holds, if it holds a single integer
#        Return None if this NestedInteger holds a nested list
#        :rtype int
#        """
#
#    def getList(self):
#        """
#        @return the nested list that this NestedInteger holds, if it holds a nested list
#        Return None if this NestedInteger holds a single integer
#        :rtype List[NestedInteger]
#        """

class Solution:
    def depthSum(self, nestedList: List[NestedInteger]) -> int:
        def _depthSum(nestedList, depth):
            nonlocal sum
            
            for ele in nestedList:
                if ele.isInteger():                
                    sum += ele.getInteger() * depth
                else:
                    _depthSum(ele.getList(), depth + 1)
        
        sum = 0
        _depthSum(nestedList, 1)
        return sum
```

## 670. Maximum Swap
```python
class Solution:
    def maximumSwap(self, num: int) -> int:
        digits = list(str(num))
        
        maxToRight = [(None, -1)] * len(digits)
        currMax = (-1, -1)
        for i in range(len(digits) - 1, -1, -1):
            currMaxDigit, currMaxIndex = currMax
            
            if int(digits[i]) < currMaxDigit:
                maxToRight[i] = currMax
                continue
                
            if int(digits[i]) > currMaxDigit:
                currMax = (int(digits[i]), i)
        
        for i in range(len(digits)):
            maxRightDigit, maxRightIndex = maxToRight[i]
            
            if maxRightDigit is not None:
                digits[i], digits[maxRightIndex] = digits[maxRightIndex], digits[i]
                return int(''.join(digits))
        
        return num
```

## 50. Pow(x, n)
```python
class Solution:
    def myPow(self, x: float, n: int) -> float:
        if x == 0:
            return 0
        
        if n < 0:
            return self.myPow(1 / x, -1 * n)

        if n == 0:
            return 1
                
        result = self.myPow(x, n // 2)
        
        if n & 1 == 1:
            return result * result * x
        else:
            return result * result
```

## 247. Strobogrammatic Number II
```python
class Solution:
    def findStrobogrammatic(self, n: int) -> List[str]:
        def _findStrobogrammatic(left, right):
            if left > right:
                result.append(''.join(path))
                return
            
            for a, b in pairs.items():
                if (a == '0' and left == 0 and n != 1 
                    or 
                    left == right and a != b):
                    continue
                    
                path[left] = a
                path[right] = b
                
                _findStrobogrammatic(left + 1, right - 1)
                
        pairs = { '0': '0', '1': '1', '6': '9', '8': '8', '9': '6' }
        
        result = []
        path = [''] * n
        _findStrobogrammatic(0, n - 1)
        return result
```

## 249. Group Shifted Strings
```python
class Solution:
    def groupStrings(self, strings: List[str]) -> List[List[str]]:
        def getGroup(string):
            group = []
            
            for char in string:
                val = (ord(char) - ord(string[0])) % 26
                group.append(str(val))
                
            return '-'.join(group)
        
        dict = collections.defaultdict(list)
        for string in strings:
            dict[getGroup(string)].append(string)
            
        return dict.values()
```

## 1060. Missing Element in Sorted Array
```python
class Solution:
    def missingElement(self, nums: List[int], k: int) -> int:
        def missingNums(mid):
            expectedNums = nums[mid] - nums[0]
            actualNums = mid
            return expectedNums - actualNums
        
        left = 1
        right = len(nums)
        
        while left < right:
            mid = (right - left) // 2 + left
            
            if missingNums(mid) < k:
                left = mid + 1
            else:
                right = mid
                
        return nums[left - 1] + k - missingNums(left - 1)
```

## 33. Search in Rotated Sorted Array
```python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left = 0
        right = len(nums) - 1
        
        while left <= right:
            mid = (right - left) // 2 + left
            
            if nums[mid] == target:
                return mid
            
            # if left of mid is sorted
            if nums[left] <= nums[mid]:
                # if target in the sorted range
                if nums[left] <= target <= nums[mid]:
                    right = mid - 1
                else:
                    left = mid + 1

            # if right of mid is sorted
            else:
                # if target in the sorted range
                if nums[mid] <= target <= nums[right]:
                    left = mid + 1
                else:
                    right = mid - 1
                    
        return -1
```

## 1762. Buildings With an Ocean View
```python
class Solution:
    def findBuildings(self, heights: List[int]) -> List[int]:
        result = []
        
        maxHeightToRight = -math.inf
        
        for i in range(len(heights) - 1, -1, -1):
            if heights[i] > maxHeightToRight:
                result.append(i)
                maxHeightToRight = heights[i]
                
        return reversed(result)
```

## 78. Subsets
### Iterative Bitmask
```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        def subsetForMask(mask):
            subset = []
            
            for i in range(len(nums)):
                if mask & 1 << i:
                    subset.append(nums[i])
            
            return subset
        
        result = []
        
        mask = 0
        bound = 2 ** len(nums)
        
        while mask < bound:
            result.append(subsetForMask(mask))
            mask += 1
        
        return result
```

### Recursive
```python
class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        def _subsets(i):
            if i >= len(nums):
                result.append(curr.copy())
                return
            
            # choose
            curr.append(nums[i])
            _subsets(i + 1)
            curr.pop()
            
            # not choose
            _subsets(i + 1)
            
        result = []
        curr = []
        _subsets(0)
        return result
```

## 348. Design Tic-Tac-Toe
```python
class TicTacToe:

    def __init__(self, n: int):
        self.n = n
        self.rows = [0] * n
        self.cols = [0] * n
        self.diag = 0
        self.antiDiag = 0

    def move(self, row: int, col: int, player: int) -> int:
        val = 1 if player == 1 else -1
        
        # Row
        self.rows[row] += val
        if self.rows[row] == val * self.n:
            return player
        
        # Col
        self.cols[col] += val
        if self.cols[col] == val * self.n:
            return player
        
        # Diag
        if row == col:
            self.diag += val
            if self.diag == val * self.n:
                return player
        
        # AntiDiag
        if row == self.n - col - 1:
            self.antiDiag += val
            if self.antiDiag == val * self.n:
                return player
            
        return 0
            
# Your TicTacToe object will be instantiated and called as such:
# obj = TicTacToe(n)
# param_1 = obj.move(row,col,player)
```

## 380. Insert Delete GetRandom O(1)
```python
class RandomizedSet:

    def __init__(self):
        self.map = {}
        self.list = []

    def insert(self, val: int) -> bool:
        if val in self.map:
            return False
        
        self.map[val] = len(self.list)
        self.list.append(val)
        
        return True

    def remove(self, val: int) -> bool:
        if val not in self.map:
            return False
        
        index = self.map[val]
        endVal = self.list[-1]
        self.list[index] = endVal
        
        self.map[endVal] = index
        self.list.pop()
        del self.map[val]
        
        return True

    def getRandom(self) -> int:
        randomIndex = random.randint(0, len(self.list) - 1)
        return self.list[randomIndex]

# Your RandomizedSet object will be instantiated and called as such:
# obj = RandomizedSet()
# param_1 = obj.insert(val)
# param_2 = obj.remove(val)
# param_3 = obj.getRandom()
```

## 143. Reorder List
```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next
class Solution:
    def reorderList(self, head: Optional[ListNode]) -> None:
        """
        Do not return anything, modify head in-place instead.
        """
        def getMiddle(head):
            fast = head.next
            slow = head
            
            while fast and fast.next:
                fast = fast.next.next
                slow = slow.next
                
            return slow
        
        def reverse(head):
            prev = None
            next = None
            
            while head:
                next = head.next
                head.next = prev
                prev = head
                head = next
                
            return prev
        
        def merge(head1, head2):
            dummy = ListNode()
            curr = dummy
            
            turn = 0
            while head1 or head2:
                if turn:
                    curr.next = head2
                    head2 = head2.next
                else:
                    curr.next = head1
                    head1 = head1.next
                    
                curr = curr.next
                turn ^= 1
                
            return dummy.next
        
        if not head.next:
            return head
        
        middleNode = getMiddle(head)
        
        secondHalfHead = middleNode.next
        middleNode.next = None
        
        reversedSecondHalfHead = reverse(secondHalfHead)
        
        merge(head, reversedSecondHalfHead)
```

## 791. Custom Sort String
```python
class Solution:
    def customSortString(self, order: str, s: str) -> str:
        count = [0] * 26
        
        result = []
        
        for char in s:
            index = ord(char) - ord('a')
            count[index] += 1
            
        for char in order:
            index = ord(char) - ord('a')
            
            while count[index]:
                result.append(char)
                count[index] -= 1
                
        for char in s:
            index = ord(char) - ord('a')
            while count[index]:
                result.append(char)
                count[index] -= 1
                
        return ''.join(result)
```

## 1379. Find a Corresponding Node of a Binary Tree in a Clone of That Tree
### DFS - Recursive Preorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def getTargetCopy(self, original: TreeNode, cloned: TreeNode, target: TreeNode) -> TreeNode:
        def dfs(node1, node2):
            if not node1:
                return None
            
            if node1 is target:
                return node2
            
            return dfs(node1.left, node2.left) or dfs(node1.right, node2.right)
            
        return dfs(original, cloned)
```

### DFS - Iterative Preorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def getTargetCopy(self, original: TreeNode, cloned: TreeNode, target: TreeNode) -> TreeNode:
        def iterTraversal(root):
            stack = [root]
            
            while stack:
                node = stack.pop()
                yield node
                
                if node.right:
                    stack.append(node.right)
                
                if node.left:
                    stack.append(node.left)
                
        for node1, node2 in zip(iterTraversal(original), iterTraversal(cloned)):
            if node1 is target:
                return node2
```

### DFS - Iterative Inorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def getTargetCopy(self, original: TreeNode, cloned: TreeNode, target: TreeNode) -> TreeNode:
        def iterTraversal(root):
            stack = []
            curr = root
            
            while stack or curr:
                while curr:
                    stack.append(curr)
                    curr = curr.left
                    
                node = stack.pop()
                yield node
                
                curr = node.right
                
        for node1, node2 in zip(iterTraversal(original), iterTraversal(cloned)):
            if node1 is target:
                return node2
```

### DFS - Iterative Postorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def getTargetCopy(self, original: TreeNode, cloned: TreeNode, target: TreeNode) -> TreeNode:
        def iterDFS(root):
            stack = [root]
            curr = root
            
            while stack or curr:
                while curr:
                    stack.append(curr)
                    curr = curr.left
                    
                if stack[-1].right:
                    curr = stack[-1].right
                    continue
                    
                node = stack.pop()
                yield node
                
                while stack and stack[-1].right is node:
                    node = stack.pop()
                    yield node
                    
        for node1, node2 in zip(iterDFS(original), iterDFS(cloned)):
            if node1 is target:
                return node2
```

### BFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

from collections import deque

class Solution:
    def getTargetCopy(self, original: TreeNode, cloned: TreeNode, target: TreeNode) -> TreeNode:
        def bfs(root):
            queue = deque([root])
            
            while queue:
                node = queue.popleft()

                yield node

                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        
        for node1, node2 in zip(bfs(original), bfs(cloned)):
            if node1 is target:
                return node2
```

### Morris - Inorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def getTargetCopy(self, original: TreeNode, cloned: TreeNode, target: TreeNode) -> TreeNode:
        def morris(root1, root2, processNodes):
            node1 = root1
            node2 = root2
            
            while node1:
                if not node1.left:
                    processNodes(node1, node2)
                    
                    node1 = node1.right
                    node2 = node2.right
                else:
                    pred1 = node1.left
                    pred2 = node2.left
                    
                    while pred1.right and pred1.right is not node1:
                        pred1 = pred1.right
                        pred2 = pred2.right
                    
                    if not pred1.right:
                        pred1.right = node1
                        pred2.right = node2
                        node1 = node1.left
                        node2 = node2.left
                    else:
                        pred1.right = None
                        pred2.right = None
                        
                        processNodes(node1, node2)
                        
                        node1 = node1.right
                        node2 = node2.right
        
        def processNodes(node1, node2):
            nonlocal found
            if node1 is target:
                found = node2
        
        found = None        
        morris(original, cloned, processNodes)
        return found
```

### Morris - Preorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

class Solution:
    def getTargetCopy(self, original: TreeNode, cloned: TreeNode, target: TreeNode) -> TreeNode:
        def morris(root1, root2, processNodes):
            node1 = root1
            node2 = root2
            
            while node1:
                if not node1.left:
                    processNodes(node1, node2)
                    
                    node1 = node1.right
                    node2 = node2.right
                else:
                    pred1 = node1.left
                    pred2 = node2.left
                    
                    while pred1.right and pred1.right is not node1:
                        pred1 = pred1.right
                        pred2 = pred2.right
                    
                    if not pred1.right:
                        processNodes(node1, node2)
                        
                        pred1.right = node1
                        pred2.right = node2
                        node1 = node1.left
                        node2 = node2.left
                    else:
                        pred1.right = None
                        pred2.right = None
                        node1 = node1.right
                        node2 = node2.right
        
        def processNodes(node1, node2):
            nonlocal found
            if node1 is target:
                found = node2
        
        found = None        
        morris(original, cloned, processNodes)
        return found
```

## 559. Maximum Depth of N-ary Tree
### DFS - Recursive Preorder
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:
    def maxDepth(self, root: 'Node') -> int:
        def dfs(root, depth = 1):
            nonlocal result
            
            if not root:
                return
            
            result = max(result, depth)
            
            for child in root.children:
                dfs(child, depth + 1)
        
        result = 0
        dfs(root)
        return result
```

### DFS - Recursive Postorder
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:
    def maxDepth(self, root: 'Node') -> int:
        def dfs(root):
            if not root:
                return 0
            
            maxDepth = 0
            for child in root.children:
                depth = dfs(child)
                maxDepth = max(maxDepth, depth)
                
            return 1 + maxDepth
        
        return dfs(root)
```

### BFS
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

from collections import deque

class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root:
            return 0
        
        queue = deque([root])
        depth = 0

        while queue:
            size = len(queue)
            
            for _ in range(size):
                node = queue.popleft()

                for child in node.children:
                    queue.append(child)

            depth += 1

        return depth
```

### DFS - Iterative Reverse Preorder
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

class Solution:
    def maxDepth(self, root: 'Node') -> int:
        if not root:
            return 0
        
        stack = [[root, 1]]
        maxDepth = 0

        while stack:
            node, depth = stack.pop()

            maxDepth = max(maxDepth, depth)

            for child in node.children:
                stack.append([child, depth + 1])

        return maxDepth
```

## 965. Univalued Binary Tree
### DFS - Recursive
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isUnivalTree(self, root: Optional[TreeNode]) -> bool:
        def dfs(node, target):
            if not node:
                return True
            
            if node.val != target:
                return False
            
            left = dfs(node.left, target)
            right = dfs(node.right, target)
            
            return left and right
                
        return dfs(root, root.val)
```

### DFS - Iterative Preorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isUnivalTree(self, root: Optional[TreeNode]) -> bool:
        def dfs(node, target):
            curr = node
            stack = []
            
            while curr or stack:
                while curr:
                    if curr.val != target:
                        return False
                    
                    stack.append(curr)
                    curr = curr.left
                    
                node = stack.pop()
                if node.right:
                    curr = node.right
                    
            return True
        
        return dfs(root, root.val)
```

### DFS - Iterative Inorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isUnivalTree(self, root: Optional[TreeNode]) -> bool:
        def dfs(node, target):
            curr = node
            stack = []
            
            while curr or stack:
                while curr:
                    stack.append(curr)
                    curr = curr.left
                    
                node = stack.pop()
                if node.val != target:
                    return False
                
                if node.right:
                    curr = node.right
                    
            return True
        
        return dfs(root, root.val)
```

### DFS - Iterative Postorder
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isUnivalTree(self, root: Optional[TreeNode]) -> bool:
        def dfs(node, target):
            curr = node
            stack = []
            
            while curr or stack:
                while curr:
                    stack.append(curr)
                    curr = curr.left
                    
                if stack[-1].right:
                    curr = stack[-1].right
                    continue
                    
                node = stack.pop()
                if node.val != target:
                    return False
                
                while stack and stack[-1].right is node:
                    node = stack.pop()
                    if node.val != target:
                        return False
                
            return True
        
        return dfs(root, root.val)
```

### Morris Traversal
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isUnivalTree(self, root: Optional[TreeNode]) -> bool:
        def morris(node, target):
            curr = node
            
            while curr:
                if not curr.left:
                    if curr.val != target:
                        return False
                    
                    curr = curr.right
                else:
                    pred = curr.left
                    while pred and pred.right and pred.right is not curr:
                        pred = pred.right
                        
                    if pred.right:
                        pred.right = None
                        curr = curr.right
                    else:
                        if curr.val != target:
                            return False
                        
                        pred.right = curr
                        curr = curr.left
                        
            return True
        
        return morris(root, root.val)
```

## 783. Minimum Distance Between BST Nodes
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

import math

class Solution:
    def minDiffInBST(self, root: Optional[TreeNode]) -> int:
        def inorder(root):
            nonlocal prev
            nonlocal diff
            
            if not root:
                return
            
            inorder(root.left)
            
            diff = min(abs(prev - root.val), diff)
            prev = root.val
            
            inorder(root.right)
            
        prev = math.inf
        diff = math.inf
        inorder(root)
        return diff
```

## 733. Flood Fill
### BFS
```python
from collections import deque

class Solution:
    def floodFill(self, image: List[List[int]], sr: int, sc: int, color: int) -> List[List[int]]:
        def fill(grid, row, col, oldColor, newColor):
            nonlocal dirs
            
            queue = deque([[row, col]])
            
            while queue:
                currRow, currCol = queue.popleft()
                
                image[currRow][currCol] = newColor
                
                for deltaRow, deltaCol in dirs:
                    nextRow = currRow + deltaRow
                    nextCol = currCol + deltaCol
                    
                    if (nextRow < 0 or nextRow >= len(image) or 
                        nextCol < 0 or nextCol >= len(image[0])):
                        continue
                    
                    if image[nextRow][nextCol] != oldColor:
                        continue
                    
                    queue.append([nextRow, nextCol])
        
        dirs = [[1, 0], [0, 1], [-1, 0], [0, -1]]
        newColor = color
        oldColor = image[sr][sc]
        
        if newColor == oldColor:
            return image
        
        fill(image, sr, sc, oldColor, newColor)
        return image
```

## 1971. Find if Path Exists in Graph
### BFS
```python
from collections import deque

class Solution:
    def validPath(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
        def bfs(start, end):
            queue = deque([start])
            visited = set([start])
            
            while queue:
                node = queue.popleft()
                
                if node == end:
                    return True
                
                for neighbor in adjMap[node]:
                    if neighbor in visited:
                        continue
                        
                    visited.add(neighbor)
                    queue.append(neighbor)
                
            return False
        
        if source == destination:
            return True
        
        # convert edge list into adjacency map
        adjMap = {}
        for u, v in edges:
            if u not in adjMap:
                adjMap[u] = []
            if v not in adjMap:
                adjMap[v] = []
        
            adjMap[u].append(v)
            adjMap[v].append(u)
        
        return bfs(source, destination)
```

### BFS - Bidirectional
```python
from collections import deque

class Solution:
    def validPath(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
        def bidirectionalBFS(start, end):
            queueS = deque([start])
            visitedS = set([start])
            
            queueE = deque([end])
            visitedE = set([end])
            
            while queueS or queueE:
                if bfs(queueS, visitedS, end):
                    return True
                
                if bfs(queueE, visitedE, start):
                    return True
            
        def bfs(queue, visited, target):
            if not queue:
                return False
            
            node = queue.popleft()
            
            if node == target:
                return True
            
            for neighbor in adjMap[node]:
                if neighbor in visited:
                    continue
                    
                visited.add(neighbor)
                queue.append(neighbor)
        
        if source == destination:
            return True
        
        # convert edge list into adjacency map
        adjMap = {}
        for u, v in edges:
            if u not in adjMap:
                adjMap[u] = []
            if v not in adjMap:
                adjMap[v] = []
        
            adjMap[u].append(v)
            adjMap[v].append(u)
        
        return bidirectionalBFS(source, destination)
```

### Union Find
```python
class Solution:
    def validPath(self, n: int, edges: List[List[int]], source: int, destination: int) -> bool:
        if source == destination:
            return True
        
        unionFind = UnionFind(n)
        
        for u, v in edges:
            unionFind.union(u, v)
            
            if unionFind.areConnected(source, destination):
                return True
            
        return False
    
class UnionFind:
    def __init__(self, n):
        self.parent = []
        self.size = []
        
        for i in range(n):
            self.parent.append(i)
            self.size.append(1)
        
    def union(self, u, v):
        parentU = self.find(u)
        parentV = self.find(v)
        
        if parentU == parentV:
            return
        
        if self.size[parentU] < self.size[parentV]:
            self.size[parentV] += self.size[parentU]
            self.parent[parentU] = parentV
        else:
            self.size[parentU] += self.size[parentV]
            self.parent[parentV] = parentU
    
    def find(self, u):
        root = u
        while root != self.parent[root]:
            root = self.parent[root]
        
        while u != root:
            next = self.parent[u]
            self.parent[u] = root
            u = next
            
        return root
    
    def areConnected(self, u, v):
        return self.find(u) == self.find(v)
```

## 1302. Deepest Leaves Sum
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def deepestLeavesSum(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        sum = 0
        
        queue = deque([root])
        while queue:
            size = len(queue)
            levelSum = 0
            for _ in range(size):
                node = queue.popleft()
                
                levelSum += node.val
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            
            sum = levelSum
            
        return sum
```

## 1315. Sum of Nodes with Even-Valued Grandparent
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def sumEvenGrandparent(self, root: TreeNode) -> int:
        def dfs(node, parent, grandparent):
            if not node:
                return 0
            
            left = dfs(node.left, node, parent)
            right = dfs(node.right, node, parent)
            
            if grandparent and grandparent.val % 2 == 0:
                return node.val + left + right
            
            return left + right
        
        return dfs(root, None, None)
```

## 1261. Find Elements in a Contaminated Binary Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class FindElements:

    def __init__(self, root: Optional[TreeNode]):
        def recoverTree(node):
            if not node:
                return
            
            self.seen.add(node.val)
            
            if node.left:
                node.left.val = 2 * node.val + 1
                recoverTree(node.left)
            if node.right:
                node.right.val = 2 * node.val + 2
                recoverTree(node.right)

        self.seen = set()
        root.val = 0
        recoverTree(root)
                
    def find(self, target: int) -> bool:
        return target in self.seen
        


# Your FindElements object will be instantiated and called as such:
# obj = FindElements(root)
# param_1 = obj.find(target)
```

## 2415. Reverse Odd Levels of Binary Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def reverseOddLevels(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        queue = deque([root])
        level = 0
        
        while queue:
            size = len(queue)
            
            nodes = []
            for _ in range(size):
                node = queue.popleft()
                
                nodes.append(node)
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            
            if level & 1:
                left = 0
                right = len(nodes) - 1
                
                while left < right:
                    temp = nodes[left].val
                    nodes[left].val = nodes[right].val
                    nodes[right].val = temp
                    
                    left += 1
                    right -= 1
                    
            level += 1
            
        return root
```

## 1448. Count Good Nodes in Binary Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def goodNodes(self, root: TreeNode) -> int:
        def dfs(node, maximum):
            if not node:
                return 0
            
            sum = 0
            
            if node.val >= maximum:
                sum += 1
              
            sum += dfs(node.left, max(maximum, node.val))
            sum += dfs(node.right, max(maximum, node.val))
            
            return sum
        
        return dfs(root, root.val)
```

## 2196. Create Binary Tree From Descriptions
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def createBinaryTree(self, descriptions: List[List[int]]) -> Optional[TreeNode]:
        treeDict = {}
        hasParent = set()
        
        for parent, child, isLeft in descriptions:
            if parent not in treeDict:
                treeDict[parent] = TreeNode(parent)
                
            if child not in treeDict:
                treeDict[child] = TreeNode(child)
            
            hasParent.add(child)
            
            if isLeft:
                treeDict[parent].left = treeDict[child]
            else:
                treeDict[parent].right = treeDict[child]
                
        for key in treeDict.keys():
            if key not in hasParent:
                return treeDict[key]
```

## 695. Max Area of Island
```python
class Solution:
    def maxAreaOfIsland(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])
        
        dirs = [[1, 0], [0, 1], [-1, 0], [0, -1]]
        
        maxArea = 0
        
        unionFind = UnionFind(m * n)
        
        for row in range(m):
            for col in range(n):
                if grid[row][col] == 1:
                    for deltaRow, deltaCol in dirs:
                        nextRow = deltaRow + row
                        nextCol = deltaCol + col
                        
                        if nextRow < 0 or nextRow >= m or nextCol < 0 or nextCol >= n:
                            continue
                            
                        if grid[nextRow][nextCol] == 0:
                            continue
                            
                        unionFind.union(row * n + col, nextRow * n + nextCol)
                    
                    maxArea = max(maxArea, unionFind.size(row * n + col))
        
        return maxArea
    
class UnionFind:
    def __init__(self, n):
        self.sizes = []
        self.parents = []
        
        for i in range(n):
            self.sizes.append(1)
            self.parents.append(i)
    
    def union(self, u, v):
        parentU = self.find(u)
        parentV = self.find(v)
        
        if parentU == parentV:
            return
        
        if self.sizes[parentU] < self.sizes[parentV]:
            self.sizes[parentV] += self.sizes[parentU]
            self.parents[parentU] = parentV
        else:
            self.sizes[parentU] += self.sizes[parentV]
            self.parents[parentV] = parentU
    
    def find(self, u):
        root = u
        while root != self.parents[root]:
            root = self.parents[root]
            
        while root != u:
            next = self.parents[u]
            self.parents[u] = root
            u = next
            
        return root
    
    def size(self, u):
        parentU = self.find(u)
        return self.sizes[parentU]
```

## 429. N-ary Tree Level Order Traversal
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val=None, children=None):
        self.val = val
        self.children = children
"""

from collections import deque

class Solution:
    def levelOrder(self, root: 'Node') -> List[List[int]]:
        if not root:
            return []
        
        queue = deque([root])
        levels = []
        while queue:
            size = len(queue)
            level = []
            
            for _ in range(size):
                node = queue.popleft()
                
                level.append(node.val)
                
                for child in node.children:
                    queue.append(child)
                    
            levels.append(level)
            
        return levels
```

## 1123. Lowest Common Ancestor of Deepest Leaves
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def lcaDeepestLeaves(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        def dfs(root, depth = 0):
            if not root:
                return [None, -1]
            
            if not root.left and not root.right:
                return [root, depth]
            
            leftNode, leftDepth = dfs(root.left, depth + 1)
            rightNode, rightDepth = dfs(root.right, depth + 1)
            
            if leftDepth == rightDepth:
                return [root, leftDepth]
            if leftDepth < rightDepth:
                return [rightNode, rightDepth]
            
            return [leftNode, leftDepth]
        
        return dfs(root)[0]
```

## 841. Keys and Rooms
```python
from collections import deque

class Solution:
    def canVisitAllRooms(self, rooms: List[List[int]]) -> bool:
        queue = deque([0])
        visited = set([0])
        
        while queue:
            node = queue.popleft()
            
            for neighbor in rooms[node]:
                if neighbor not in visited:
                    visited.add(neighbor)
                    queue.append(neighbor)
                    
        return len(visited) == len(rooms)
```

## 865. Smallest Subtree with all the Deepest Nodes
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def subtreeWithAllDeepest(self, root: TreeNode) -> TreeNode:
        def dfs(root, depth = 0):
            if not root:
                return [None, -1]
            
            if not root.left and not root.right:
                return [root, depth]
            
            leftNode, leftDepth = dfs(root.left, depth + 1)
            rightNode, rightDepth = dfs(root.right, depth + 1)
            
            if leftDepth == rightDepth:
                return [root, leftDepth]
            if leftDepth < rightDepth:
                return [rightNode, rightDepth]
            
            return [leftNode, leftDepth]
        
        return dfs(root)[0]
```

## 1992. Find All Groups of Farmland
```python
class Solution:
    def findFarmland(self, land: List[List[int]]) -> List[List[int]]:
        def dfs(row, col):
            # if out of bounds return
            if row < 0 or row >= m or col < 0 or col >= n:
                return [-1, -1]

            # if framland return
            if land[row][col] == 0:
                return [-1, -1]

            # if visited return
            if (row, col) in visited:
                return [-1, -1]

            visited.add((row, col))

            # go to neighbors
            rowDown, colDown = dfs(row + 1, col)
            rowRight, colRight = dfs(row, col + 1)
            
            return [max(row, rowDown, rowRight), max(col, colDown, colRight)]
            
            
        m = len(land)
        n = len(land[0])
        
        result = []
        visited = set()
        
        for row in range(m):
            for col in range(n):
                if land[row][col] == 1 and (row, col) not in visited:
                    row2, col2 = dfs(row, col)
                    result.append([row, col, row2, col2])
                    
        return result
```

## 1457. Pseudo-Palindromic Paths in a Binary Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def pseudoPalindromicPaths (self, root: Optional[TreeNode]) -> int:
        def dfs(root, bits = 0):
            if not root:
                return 0
            
            bits ^= 1 << root.val
            
            if not root.left and not root.right and bits & bits - 1 == 0:
                return 1
            
            return dfs(root.left, bits) + dfs(root.right, bits)
        
        return dfs(root)
```

## 513. Find Bottom Left Tree Value
### BFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        queue = deque([root])
        
        leftMost = 0
        
        while queue:
            size = len(queue)
            for i in range(size):
                node = queue.popleft()
                
                if i == 0:
                    leftMost = node.val
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
        
        return leftMost
```

### DFS
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def findBottomLeftValue(self, root: Optional[TreeNode]) -> int:
        def dfs(root, row = 0):
            nonlocal leftMost
            nonlocal leftMostRow
            
            if not root:
                return
            
            if not root.left and not root.right:
                if row > leftMostRow:
                    leftMostRow = row
                    leftMost = root.val
                    
                return
                    
            dfs(root.left, row + 1)
            dfs(root.right, row + 1)
        
        leftMost = root.val
        leftMostRow = 0
        
        dfs(root)
        
        return leftMost
```

## 1161. Maximum Level Sum of a Binary Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def maxLevelSum(self, root: Optional[TreeNode]) -> int:
        queue = deque([root])
        
        level = 1
        
        smallestLevel = 1
        maxSum = -math.inf
        
        while queue:
            size = len(queue)
            sum = 0
            for _ in range(size):
                node = queue.popleft()
                
                sum += node.val
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                    
            if maxSum < sum:
                maxSum = sum
                smallestLevel = level
                
            level += 1
                    
        return smallestLevel
```

## 959. Regions Cut By Slashes
```python
class Solution:
    def regionsBySlashes(self, grid: List[str]) -> int:
        def fillExpandedGrid(grid, expandedGrid, expandedMultipler):
            n = len(grid)
            
            for row in range(n):
                for col in range(n):
                    expandedRow = row * expandedMultipler
                    expandedCol = col * expandedMultipler
                    
                    if grid[row][col] == "/":
                        currRow = expandedRow
                        currCol = expandedCol + expandedMultipler - 1
                        while currRow < expandedRow + expandedMultipler:
                            expandedGrid[currRow][currCol] = 1
                            currRow += 1
                            currCol -= 1                            
                    elif grid[row][col] == "\\":
                        currRow = expandedRow
                        currCol = expandedCol
                        while currRow < expandedRow + expandedMultipler:
                            expandedGrid[currRow][currCol] = 1
                            currRow += 1
                            currCol += 1
        
        
        def countRegions(expandedGrid):
            n = len(expandedGrid)
            m = len(expandedGrid[0])
            dirs = [[1, 0], [0, 1], [-1, 0], [0, -1]]
            
            unionFind = UnionFind(n * m)
            onesCount = 0
            
            for row in range(n):
                for col in range(m):
                    if expandedGrid[row][col] == 1:
                        onesCount += 1
                        continue
                        
                    for deltaRow, deltaCol in dirs:
                        nextRow = deltaRow + row
                        nextCol = deltaCol + col

                        if (nextRow < 0 or nextRow >= n or
                            nextCol < 0 or nextCol >= m):
                            continue
                            
                        if expandedGrid[nextRow][nextCol] == 1:
                            continue

                        unionFind.union(row * m + col, nextRow * m + nextCol)
                        
            return unionFind.numOfComponents - onesCount
            
            
        n = len(grid)
        expandedMultipler = 3
        expandedGrid = [[0] * n * expandedMultipler for i in range(n * expandedMultipler)]
        
        fillExpandedGrid(grid, expandedGrid, expandedMultipler)
        
        return countRegions(expandedGrid)
    
    
class UnionFind:
    def __init__(self, n):
        self.parents = []
        self.sizes = []

        self.numOfComponents = n
        
        for i in range(n):
            self.parents.append(i)
            self.sizes.append(1)
            
    def union(self, a, b):
        parentA = self.find(a)
        parentB = self.find(b)
        
        if parentA == parentB:
            return
        
        if self.sizes[parentA] < self.sizes[parentB]:
            self.sizes[parentB] += self.sizes[parentA]
            self.parents[parentA] = parentB
        else:
            self.sizes[parentA] += self.sizes[parentB]
            self.parents[parentB] = parentA
        
        self.numOfComponents -= 1
        
    def find(self, a):
        root = a
        
        while root != self.parents[root]:
            root = self.parents[root]
            
        while a != root:
            next = self.parents[a]
            self.parents[a] = root
            a = next
            
        return root
```

## 1905. Count Sub Islands
```python
class Solution:
    def countSubIslands(self, grid1: List[List[int]], grid2: List[List[int]]) -> int:
        dirs = [[1, 0], [0, 1]]
        
        m = len(grid1)
        n = len(grid1[0])
        
        unionFind = UnionFind(m * n)
        
        for row in range(m):
            for col in range(n):
                if grid1[row][col] == 0:
                    continue
                    
                for deltaRow, deltaCol in dirs:
                    nextRow = deltaRow + row
                    nextCol = deltaCol + col
                    
                    if (nextRow < 0 or nextRow >= m or 
                        nextCol < 0 or nextCol >= n):
                        continue
                        
                    if grid1[nextRow][nextCol] == 0:
                        continue
                    
                    unionFind.union(row * n + col, nextRow * n + nextCol)
        
        
        subCount = 0
        for row in range(m):
            for col in range(n):
                if grid1[row][col] == 0 or grid2[row][col] == 0:
                    continue
                    
                if isSubIsland(grid2, row, col, unionFind):
                    subCount += 1
                
        return subCount
    
    
def isSubIsland(grid, row, col, unionFind):
    def dfs(row, col):
        if (row < 0 or row >= len(grid) or 
            col < 0 or col >= len(grid[0])):
            return True
        
        if grid[row][col] == 0:
            return True
        
        grid[row][col] = 0
        
        result = True
        for deltaRow, deltaCol in dirs:
            nextRow = deltaRow + row
            nextCol = deltaCol + col
            
            if not dfs(nextRow, nextCol):
                result = False
                
        return result and unionFind.find(row * n + col) == parent
        
    dirs = [[1, 0], [0, 1], [-1, 0], [0, -1]]
    m = len(grid)
    n = len(grid[0])
    
    parent = unionFind.find(row * n + col)
    return dfs(row, col)


class UnionFind:
    def __init__(self, n):
        self.parents = []
        self.sizes = []
        
        for i in range(n):
            self.parents.append(i)
            self.sizes.append(1)
            
        self.numOfComponents = n
            
    def union(self, a, b):
        parentA = self.find(a)
        parentB = self.find(b)
        
        if parentA == parentB:
            return
        
        if self.sizes[parentA] < self.sizes[parentB]:
            self.sizes[parentB] += self.sizes[parentA]
            self.parents[parentA] = parentB
        else:
            self.sizes[parentA] += self.sizes[parentB]
            self.parents[parentB] = parentA
            
        self.numOfComponents -= 1
    
    def find(self, a):
        root = a
        
        while root != self.parents[root]:
            root = self.parents[root]
            
        while a != root:
            next = self.parents[a]
            self.parents[a] = root
            a = next
            
        return root
```

## 690. Employee Importance
```python
"""
# Definition for Employee.
class Employee:
    def __init__(self, id: int, importance: int, subordinates: List[int]):
        self.id = id
        self.importance = importance
        self.subordinates = subordinates
"""

from collections import deque

class Solution:
    def getImportance(self, employees: List['Employee'], id: int) -> int:
        def bfs(root):
            totalImportance = 0
            
            queue = deque([root])
            while queue:
                node = queue.popleft()
                
                totalImportance += node["importance"]
                
                for subordinate in node["subordinates"]:
                    queue.append(employeeDict[subordinate])
                
            return totalImportance
            
        
        employeeDict = {}
        for employee in employees:
            employeeDict[employee.id] = {"importance": employee.importance, 
                                         "subordinates": employee.subordinates}
                                         
        if id not in employeeDict:
            return 0
        
        return bfs(employeeDict[id])
```

## 529. Minesweeper
```python
from collections import deque

class Solution:
    def updateBoard(self, board: List[List[str]], click: List[int]) -> List[List[str]]:
        # next click position among all the unrevealed squares ('M' or 'E').
        clickRow, clickCol = click
        
        # If a mine 'M' is revealed, then the game is over. You should change it to 'X'.
        if board[clickRow][clickCol] == 'M':
            board[clickRow][clickCol] = 'X'
            return board
        
        dirs = [[1, 0], [0, 1], [-1, 0], [0, -1], [1, 1], [-1, -1], [-1, 1], [1, -1]]
        queue = deque([click])
        while queue:
            currRow, currCol = queue.popleft()
            
            if board[currRow][currCol] not in 'EM':
                continue
                            
            # all of its adjacent unrevealed squares should be revealed recursively.
            # If an empty square 'E' with at least one adjacent mine is revealed, 
            # then change it to a digit ('1' to '8') representing the number of adjacent mines.
            
            mineCount = 0
            for deltaRow, deltaCol in dirs:
                nextRow = deltaRow + currRow
                nextCol = deltaCol + currCol
                
                if (nextRow < 0 or nextRow >= len(board) or 
                    nextCol < 0 or nextCol >= len(board[0])):
                    continue
                    
                if board[nextRow][nextCol] == "M":
                    mineCount += 1
                    continue
                                        
            # If an empty square 'E' with no adjacent mines is revealed, 
            # then change it to a revealed blank 'B'
            if board[currRow][currCol] == 'E':
                if mineCount == 0:
                    board[currRow][currCol] = 'B'
                else:
                    board[currRow][currCol] = str(mineCount)
                    continue
            
            
            for deltaRow, deltaCol in dirs:
                nextRow = deltaRow + currRow
                nextCol = deltaCol + currCol
                
                if (nextRow < 0 or nextRow >= len(board) or 
                    nextCol < 0 or nextCol >= len(board[0])):
                    continue
                
                if board[nextRow][nextCol] == 'E':
                    queue.append([nextRow, nextCol])
            
        return board
```

## 919. Complete Binary Tree Inserter
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class CBTInserter:

    def __init__(self, root: Optional[TreeNode]):
        self.root = root
        self.tree = deque()
        
        queue = deque([root])
        while queue:
            node = queue.popleft()
            
            if not node.left or not node.right:
                self.tree.append(node)
                
            if node.left:
                queue.append(node.left)
            if node.right:
                queue.append(node.right)
                

    def insert(self, val: int) -> int:
        node = TreeNode(val)
        self.tree.append(node)
        
        parent = self.tree[0]
        
        if not parent.left:
            parent.left = node
        else:
            parent.right = node
            self.tree.popleft()
            
        return parent.val

    def get_root(self) -> Optional[TreeNode]:
        return self.root
    
    
# Your CBTInserter object will be instantiated and called as such:
# obj = CBTInserter(root)
# param_1 = obj.insert(val)
# param_2 = obj.get_root()
```

## 515. Find Largest Value in Each Tree Row
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque
import math

class Solution:
    def largestValues(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        result = []
        queue = deque([root])
        
        while queue:
            size = len(queue)
            maxForRow = -math.inf
            for _ in range(size):
                node = queue.popleft()
                
                maxForRow = max(maxForRow, node.val)
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                    
            result.append(maxForRow)
        
        return result
```

## 1020. Number of Enclaves
### BFS
```python
from collections import deque

class Solution:
    def numEnclaves(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])
        
        count = 0
        for row in range(m):
            for col in range(n):
                if grid[row][col] == 1:
                    result = self.isEnclave(grid, row, col)
                    count += result
        
        return count

    
    def isEnclave(self, grid, row, col):
        m = len(grid)
        n = len(grid[0])
        
        dirs = [[1, 0], [0, 1], [-1, 0], [0, -1]]
        
        count = 1
        queue = deque([[row, col]])
        grid[row][col] = 0
        
        while queue:
            currRow, currCol = queue.popleft()
            
            for deltaRow, deltaCol in dirs:
                nextRow = deltaRow + currRow
                nextCol = deltaCol + currCol

                if nextRow < 0 or nextRow >= m or nextCol < 0 or nextCol >= n:
                    count = -1
                    continue
                
                if grid[nextRow][nextCol] == 1:
                    grid[nextRow][nextCol] = 0
                    
                    if count != -1:
                        count += 1
                        
                    queue.append([nextRow, nextCol])
                    
        if count == -1:
            return 0
        else:
            return count
```

## 547. Number of Provinces
```python
class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        n = len(isConnected)
        
        unionFind = UnionFind(n)
        
        for i in range(n):
            for j in range(n):
                if isConnected[i][j] == 1:
                    unionFind.union(i, j)
                    
        return unionFind.numOfComponents
    
class UnionFind:
    def __init__(self, size):
        self.size = size
        self.parents = []
        self.sizes = []
        self.numOfComponents = size
        
        for i in range(size):
            self.parents.append(i)
            self.sizes.append(1)
            
    def union(self, a, b):
        parentA = self.find(a)
        parentB = self.find(b)
        
        if parentA == parentB:
            return
        
        if self.sizes[parentA] < self.sizes[parentB]:
            self.sizes[parentB] += self.sizes[parentA]
            self.parents[parentA] = parentB
        else:
            self.sizes[parentA] += self.sizes[parentB]
            self.parents[parentB] = parentA
        
        self.numOfComponents -= 1
    
    def find(self, a):
        root = a
        while root != self.parents[root]:
            root = self.parents[root]
            
        while a != root:
            next = self.parents[a]
            self.parents[a] = root
            a = self.parents[a]
        
        return root
```

## 102. Binary Tree Level Order Traversal
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        
        levels = []
        queue = deque([root])
        while queue:
            size = len(queue)
            level = []
            for _ in range(size):
                node = queue.popleft()
                
                level.append(node.val)
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                    
            levels.append(level)
            
        return levels
```

## 1306. Jump Game III
```python
from collections import deque

class Solution:
    def canReach(self, arr: List[int], start: int) -> bool:
        queue = deque([start])
        visited = set([start])
        
        while queue:
            index = queue.popleft()
            
            if arr[index] == 0:
                return True
            
            # option 1
            option1 = index + arr[index]
            # skip if out of bounds, skip if visited
            if option1 < len(arr) and option1 not in visited:
                visited.add(option1)
                queue.append(option1)
                
            # option 2
            option2 = index - arr[index]
            # skip if out of bounds, skip if visited
            if 0 <= option2 and option2 not in visited:
                visited.add(option2)
                queue.append(option2)
                
        return False
```

## 863. All Nodes Distance K in Binary Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None

from collections import deque

class Solution:
    def distanceK(self, root: TreeNode, target: TreeNode, k: int) -> List[int]:
        def bfs(start, k):
            queue = deque([start.val])
            visited = set([start.val])
            
            result = []
            level = 0
            while queue:
                size = len(queue)
                for _ in range(size):
                    node = queue.popleft()
                    result.append(node)
                    
                    for neighbor in graph[node]:
                        if neighbor in visited:
                            continue
                        
                        visited.add(neighbor)
                        queue.append(neighbor)
                
                if level == k:
                    return result
                
                level += 1
                result = []
                    
            return []
        
        
        def dfs(node, parent):
            if not node:
                return
            
            if node.val not in graph:
                graph[node.val] = []
                
            if parent and parent.val not in graph:
                graph[parent.val] = []
            
            if parent:
                graph[parent.val].append(node.val)
                graph[node.val].append(parent.val)
            
            dfs(node.left, node)
            dfs(node.right, node)
            
        graph = {}
        dfs(root, None)
        return bfs(target, k)
```

## 684. Redundant Connection
```python
class Solution:
    def findRedundantConnection(self, edges: List[List[int]]) -> List[int]:
        n = len(edges)
        
        unionFind = UnionFind(n + 1)
        
        for a, b in edges:
            if unionFind.isConnected(a, b):
                return [a, b]
            
            unionFind.union(a, b)
            
        return []
    
class UnionFind:
    def __init__(self, size):
        self.size = size
        self.sizes = []
        self.parents = []
        
        for i in range(self.size):
            self.sizes.append(1)
            self.parents.append(i)
            
    def union(self, a, b):
        parentA = self.find(a)
        parentB = self.find(b)
        
        if parentA == parentB:
            return
        
        if self.sizes[parentA] < self.sizes[parentB]:
            self.sizes[parentB] += self.sizes[parentA]
            self.parents[parentA] = parentB
        else:
            self.sizes[parentA] += self.sizes[parentB]
            self.parents[parentB] = parentA
    
    def find(self, a):
        root = a
        while root != self.parents[root]:
            root = self.parents[root]
            
        while a != root:
            next = self.parents[a]
            self.parents[a] = root
            a = next
            
        return root
    
    def isConnected(self, a, b):
        return self.find(a) == self.find(b)
```

## 655. Print Binary Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def printTree(self, root: Optional[TreeNode]) -> List[List[str]]:
        if not root:
            return []
        
        height = -1
        queue = deque([root])
        while queue:
            size = len(queue)
            for _ in range(size):
                node = queue.popleft()
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            height += 1
        
        
        m = height + 1
        n = (2 ** (height + 1)) - 1
        
        result = [[""] * n for i in range(m)]
        
        queue = deque([[root, 0, ((n - 1) // 2)]])
        while queue:
            node, row, col = queue.popleft()
            result[row][col] = str(node.val)
            
            if node.left:
                queue.append([node.left, row + 1, col - 2 ** (height - row - 1)])
            if node.right:
                queue.append([node.right, row + 1, col + 2 ** (height - row - 1)])
        
            
        return result
```

## 199. Binary Tree Right Side View
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def rightSideView(self, root: Optional[TreeNode]) -> List[int]:
        if not root:
            return []
        
        result = []
        queue = deque([root])
        while queue:
            size = len(queue)
                
            for i in range(size):
                node = queue.popleft()

                if i == size - 1:
                    result.append(node.val)
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                    
        return result
```

## 107. Binary Tree Level Order Traversal II
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
from collections import deque

class Solution:
    def levelOrderBottom(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        
        levels = []
        queue = deque([root])
        while queue:
            size = len(queue)
            level = []
            for i in range(size):
                node = queue.popleft()
                
                level.append(node.val)
                
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                    
            levels.append(level)
               
        levels.reverse()
        return levels
```

## 116. Populating Next Right Pointers in Each Node
```python
"""
# Definition for a Node.
class Node:
    def __init__(self, val: int = 0, left: 'Node' = None, right: 'Node' = None, next: 'Node' = None):
        self.val = val
        self.left = left
        self.right = right
        self.next = next
"""

from collections import deque

class Solution:
    def connect(self, root: 'Optional[Node]') -> 'Optional[Node]':
        if not root:
            return root
        
        queue = deque([root])
        
        while queue:
            size = len(queue)
            prev = None
            for i in range(size):
                node = queue.popleft()
                
                if prev:
                    prev.next = node
                    
                prev = node
                    
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
                    
        return root
```

## 1267. Count Servers that Communicate
```python
class Solution:
    def countServers(self, grid: List[List[int]]) -> int:
        m = len(grid)
        n = len(grid[0])
        
        totalServers = set()
        
        for row in range(m):
            rowServers = set()
            for col in range(n):
                if grid[row][col] == 1:
                    rowServers.add((row, col))
                    
            if len(rowServers) > 1:
                totalServers |= rowServers
                    
        for col in range(n):
            colServers = set()
            for row in range(m):
                if grid[row][col] == 1:
                    colServers.add((row, col))
                    
            if len(colServers) > 1:
                totalServers |= colServers
                    
        return len(totalServers)
```

## 1376. Time Needed to Inform All Employees
```python
from collections import deque

class Solution:
    def numOfMinutes(self, n: int, headID: int, manager: List[int], informTime: List[int]) -> int:
        graph = {}
        for (employee, boss) in enumerate(manager):
            if employee not in graph:
                graph[employee] = []
            
            if boss == -1:
                continue
                
            if boss not in graph:
                graph[boss] = []
            
            graph[boss].append(employee)
        
        
        maxTime = 0
        queue = deque([[headID, informTime[headID]]])
        while queue:
            node, time = queue.popleft()
            
            maxTime = max(maxTime, time)
            
            for neighbor in graph[node]:
                queue.append([neighbor, time + informTime[neighbor]])
                
        return maxTime
```

## 1319. Number of Operations to Make Network Connected
```python
class Solution:
    def makeConnected(self, n: int, connections: List[List[int]]) -> int:
        unionFind = UnionFind(n)
        
        redundantConnections = 0
        for a, b in connections:
            if unionFind.isConnected(a, b):
                redundantConnections += 1
                continue
                
            unionFind.union(a, b)
            
        isolatedMachineCount = unionFind.numOfComponents - 1
        if redundantConnections < isolatedMachineCount:
            return -1
        
        return isolatedMachineCount

class UnionFind:
    def __init__(self, size):
        self.size = size
        self.numOfComponents = size
        self.sizes = []
        self.parents = []
        
        for i in range(size):
            self.sizes.append(1)
            self.parents.append(i)
            
    def union(self, a, b):
        parentA = self.find(a)
        parentB = self.find(b)
        
        if parentA == parentB:
            return
            
        if self.sizes[parentA] < self.sizes[parentB]:
            self.sizes[parentB] += self.sizes[parentA]
            self.parents[parentA] = parentB
        else:
            self.sizes[parentA] += self.sizes[parentB]
            self.parents[parentB] = parentA
        
        self.numOfComponents -= 1
    
    def find(self, a):
        root = a
        while root != self.parents[root]:
            root = self.parents[root]
            
        while a != root:
            next = self.parents[a]
            self.parents[a] = root
            a = next
            
        return root
    
    def isConnected(self, a, b):
        return self.find(a) == self.find(b)
```