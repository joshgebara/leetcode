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
            numOfFives += n // 5
            n = n // 5
        
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
        def getRandomIndex(low, high):
            return math.trunc((high - low + 1) * random.random()) + low
        
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
            randomIndex = getRandomIndex(left, right)
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