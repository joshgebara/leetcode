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