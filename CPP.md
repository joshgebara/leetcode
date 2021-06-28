# Leetcode C++ Solutions

## 53. Maximum Subarray
### Kadane's Algorithm
```cpp
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if (nums.size() == 0) {
            return 0;
        }
        
        int globalMax = nums[0];
        int localMax = nums[0];
        
        for (auto i = 1; i < nums.size(); ++i) {
            localMax = max(localMax + nums[i], nums[i]);
            globalMax = max(localMax, globalMax);
        }
        
        return globalMax; 
    }
};
```

## 206. Reverse Linked List
### Iterative
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* prev = nullptr;
        ListNode* next = nullptr;
        
        while (head != nullptr) {
            next = head->next;
            head->next = prev;
            prev = head;
            head = next;
        }
        
        return prev;
    }
};
```

### Recursive
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        if (head == nullptr || head->next == nullptr) {
            return head;
        }
        
        ListNode* reversedHead = reverseList(head->next);
        head->next->next = head;
        head->next = nullptr;
            
        return reversedHead;
    }
};
```

## 412. Fizz Buzz
```cpp
class Solution {
public:
    vector<string> fizzBuzz(int n) {
        vector<string> result;
        
        for (auto i = 1; i <= n; ++i) {
            string str;
            
            if (i % 3 == 0) {
                str += "Fizz";
            }
            
            if (i % 5 == 0) {
                str += "Buzz";
            }
            
            if (str.empty()) {
                str += to_string(i);
            }
            
            result.push_back(str);
        }
        
        return result;
    }
};
```

## 283. Move Zeroes
```cpp
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int i = 0;
        
        for (auto j = 0; j < nums.size(); ++j) {
            if (nums[j] == 0) {
                continue;
            }
            
            swap(nums[i], nums[j]);
            ++i;
        }
    }
};
```