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
        map = { '6': '9', '9': '6', '8': '8', '0': '0', '1': '1' }
        
        left = 0
        right = len(num) - 1
        
        while (left <= right):
            if not num[left] in map:
                return False
            
            if not num[right] in map:
                return False
            
            if (num[left] != map[num[right]]):
                return False
            
            left += 1
            right -=1
            
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
        counter1 = Counter(nums1)
        counter2 = Counter(nums2)
        
        result = []
        
        for num in set(nums1):
            if num in counter1 and num in counter2:
                for i in range(min(counter1[num], counter2[num])):
                    result.append(num)
        
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
        result = []
        
        left = 0
        right = len(nums) - 1
        
        while left <= right:
            leftSquared = nums[left] ** 2
            rightSquared = nums[right] ** 2
            
            if leftSquared < rightSquared:
                result.append(rightSquared)
                right -=1
            else:
                result.append(leftSquared)
                left += 1
        
        return result[::-1]
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
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def minDepth(self, root: TreeNode) -> int:
        if root is None:
            return 0
        
        left = self.minDepth(root.left)    
        right = self.minDepth(root.right)
        
        if left == 0:
            return 1 + right
        
        if right == 0:
            return 1 + left
        
        return 1 + min(left, right)
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
        right = num
        
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
```python
class Solution:
    def missingNumber(self, nums: List[int]) -> int:
        n = len(nums)
        expectedSum = n * (n + 1) // 2
        actualSum = sum(nums)
        return expectedSum - actualSum
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
        self.elements = []
        self.mins = []
        
    def push(self, val: int) -> None:
        self.elements.append(val)
        
        if self.mins:
            self.mins.append(min(self.mins[-1], val))
        else:
            self.mins.append(val)

    def pop(self) -> None:
        self.elements.pop()
        self.mins.pop()
            
    def top(self) -> int:
        return self.elements[-1]

    def getMin(self) -> int:
        return self.mins[-1]


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

## 70. Climbing Stairs
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

## 993. Cousins in Binary Tree
```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def isCousins(self, root: Optional[TreeNode], x: int, y: int) -> bool:
        queue = deque([(root, None)])
        
        while queue:
            size = len(queue)
            xParent = None
            yParent = None
            
            for i in range(size):
                node, parent = queue.popleft()
                
                if node.val == x:
                    xParent = parent  
                if node.val == y:
                    yParent = parent
                
                if node.left:
                    queue.append((node.left, node))
                if node.right:
                    queue.append((node.right, node))
            
            if xParent and yParent and xParent != yParent:
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
        result = []
        for num in range(1, n + 1):
            str = ''
            if num % 3 == 0:
                str += 'Fizz'
            
            if num % 5 == 0:
                str += 'Buzz'
            
            if not len(str):
                str += f'{num}'
            
            result.append(str)
            
        return result
```

## 509. Fibonacci Number
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
        self.flip(image)
        self.invert(image)
        return image
    
    def flip(self, image):
        for row in image:
            row.reverse()
    
    def invert(self, image):
        for row in range(len(image)):
            for col in range(len(image[row])):
                image[row][col] ^= 1
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

## 645. Set Mismatch
```python
class Solution:
    def findErrorNums(self, nums: List[int]) -> List[int]:
        map = {}
        
        for num in range(1, len(nums) + 1):
            map[num] = 0
            
        for num in nums:
            map[num] += 1
        
        occurZero = -1
        occurTwice = -1
        
        for key in map:
            if map[key] == 0:
                occurZero = key
            if map[key] == 2:
                occurTwice = key
                
        return [occurTwice, occurZero]
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