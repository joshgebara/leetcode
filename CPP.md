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

## 21. Merge Two Sorted Lists
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
    ListNode* mergeTwoLists(ListNode* l1, ListNode* l2) {
        ListNode dummy;
        ListNode* curr = &dummy;
        
        while (l1 && l2) {
            if (l1->val < l2->val) {
                curr->next = l1;
                l1 = l1->next;
            } else {
                curr->next = l2;
                l2 = l2->next;
            }
            curr = curr->next;
        }
        
        if (l1) {
            curr->next = l1;
        } else {
            curr->next = l2;
        }
        
        return dummy.next;
    }
};
```

## 965. Univalued Binary Tree
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode() : val(0), left(nullptr), right(nullptr) {}
 *     TreeNode(int x) : val(x), left(nullptr), right(nullptr) {}
 *     TreeNode(int x, TreeNode *left, TreeNode *right) : val(x), left(left), right(right) {}
 * };
 */
class Solution {
public:
    bool isUnivalTree(TreeNode* root) {
        return dfs(root, root->val);
    }
private:
    bool dfs(TreeNode* root, int rootVal) {
        if (!root) {
            return true;
        }
        
        if (root->val != rootVal) {
            return false;
        }
        
        bool left = dfs(root->left, rootVal);
        bool right = dfs(root->right, rootVal);
        return left && right;
    }
};
```

## 1636. Sort Array by Increasing Frequency
```cpp
class Solution {
public:
    vector<int> frequencySort(vector<int>& nums) {
        unordered_map<int, int> map;
        
        for (int num : nums) {
            ++map[num];
        }
        
        sort(begin(nums), end(nums), [&](int a, int b) {
            if (map[a] == map[b]) {
                return a > b;
            }
            
            return map[a] < map[b];
        });
        
        return nums;
    }
};
```

## 232. Implement Queue using Stacks
```cpp
class MyQueue {
public:    
    /** Initialize your data structure here. */
    MyQueue() {
        
    }
    
    /** Push element x to the back of queue. */
    void push(int x) {
        stack1.push(x);    
    }
    
    /** Removes the element from in front of queue and returns that element. */
    int pop() {
        balance();
        
        int temp = stack2.top();
        stack2.pop();
        return temp;
    }
    
    /** Get the front element. */
    int peek() {
        balance();
        
        return stack2.top();
    }
    
    /** Returns whether the queue is empty. */
    bool empty() {
        return stack1.empty() && stack2.empty();
    }
private:
    stack<int> stack1;
    stack<int> stack2;
    
    void balance() {
        if (stack2.empty()) {
            while (stack1.size()) {
                stack2.push(stack1.top());
                stack1.pop();
            }
        }
    }
};

/**
 * Your MyQueue object will be instantiated and called as such:
 * MyQueue* obj = new MyQueue();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->peek();
 * bool param_4 = obj->empty();
 */
```

## 14. Longest Common Prefix
```cpp
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {        
        string result;
        
        for (auto charIndex = 0; charIndex < strs[0].size(); ++charIndex) {
            char target = strs[0][charIndex];
            for (auto strIndex = 1; strIndex < strs.size(); ++strIndex) {
                if (strs[strIndex].size() < charIndex || 
                    strs[strIndex][charIndex] != target) {
                    return result;
                }
            }
            
            result += target;
        }
        
        return result;
    }
};
```

## 977. Squares of a Sorted Array
```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int> result(nums.size());
        int left = 0;
        int right = nums.size() - 1;
        
        for (int index = nums.size() - 1; index >= 0; --index) {
            int leftSquared = pow(nums[left], 2);
            int rightSquared = pow(nums[right], 2);
            
            if (leftSquared > rightSquared) {
                result[index] = leftSquared;
                ++left;
            } else {
                result[index] = rightSquared;
                --right;
            }
        }
        
        return result;
    }
};
```

## 20. Valid Parentheses
```cpp
class Solution {
public:
    bool isValid(string s) {    
        stack<char> stack;
        
        for (auto c : s) {
            if (map[c]) {
                stack.push(c);
                continue;
            }
            
            if (stack.empty() || map[stack.top()] != c) {
                return false;
            }

            stack.pop();
        }
        
        return stack.empty();
    }
    
private:
    unordered_map<char, char> map = { {'(',')'}, {'{','}'}, {'[', ']'} };
};
```

## 349. Intersection of Two Arrays
```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> set(nums1.begin(), nums1.end());
        unordered_set<int> intersectionSet;
        
        for (auto num : nums2) {
            if (set.find(num) != set.end()) {
                intersectionSet.insert(num);
            }
        }
        
        vector<int> result(intersectionSet.begin(), intersectionSet.end());
        return result;
    }
};
```

## 169. Majority Element
```cpp
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int candidate = nums[0];
        int count = 1;
        
        for (auto index = 1; index < nums.size(); ++index) {
            if (candidate == nums[index]) {
                ++count;
                continue;
            }
            
            --count;
                
            if (count == 0) {
                candidate = nums[index];
                count = 1;
            }
        }
        
        return candidate;
    }
};
```

## 225. Implement Stack using Queues
```cpp
class MyStack {
public:    
    /** Initialize your data structure here. */
    MyStack() {
        
    }
    
    /** Push element x onto stack. */
    void push(int x) {
        queueTop.push(x);
    }
    
    /** Removes the element on top of the stack and returns that element. */
    int pop() {
        rebalance();
        
        int front = queueTop.front();
        queueTop.pop();
        return front;
    }
    
    /** Get the top element. */
    int top() {
        rebalance();
        
        int front = queueTop.front();
        return front;
    }
    
    /** Returns whether the stack is empty. */
    bool empty() {
        return queueTop.empty() && queueRemaining.empty();
    }
private:
    queue<int> queueTop;
    queue<int> queueRemaining;
    
    void rebalance() {
        if (queueTop.empty()) {
            swap(queueTop, queueRemaining);
        }
        
        while (queueTop.size() > 1) {
            int front = queueTop.front();
            queueTop.pop();
            queueRemaining.push(front);
        }
    }
};

/**
 * Your MyStack object will be instantiated and called as such:
 * MyStack* obj = new MyStack();
 * obj->push(x);
 * int param_2 = obj->pop();
 * int param_3 = obj->top();
 * bool param_4 = obj->empty();
 */
```

## 342. Power of Four
### Logs
```cpp
class Solution {
public:
    bool isPowerOfFour(int n) {
        if (n <= 0) return false;
        if (n == 1) return true;
        
        int power = log(n) / log(4);
        int result = pow(4, power);
        return result == n;
    }
};
```

### Bit Manipulation
```cpp
class Solution {
public:
    bool isPowerOfFour(int n) {
        if (n <= 0) {
            return false;
        }
        
        if ((n & (n - 1)) != 0) {
            return false;
        }
        
        return (n & 0x55555555) == n;
    }
};
```

## 326. Power of Three
```cpp
class Solution {
public:
    bool isPowerOfThree(int n) {
        if (n <= 0) return false;
        if (n == 1) return true;
        
        int power = log10(n) / log10(3);
        int result = pow(3, power);
        return result == n;
    }
};
```

## 66. Plus One
```cpp
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int carry = 1;
        
        for (int i = digits.size() - 1; i >= 0; --i) {
            int sum = digits[i] + carry;
            digits[i] = sum % 10;
            carry = sum / 10;
        }
        
        if (carry > 0) {
            digits.insert(digits.begin(), carry);
        }
        
        return digits;
    }
};
```

## 1. Two Sum
```cpp
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> map;
        
        for (int i = 0; i < nums.size(); ++i) {
            int candidate = target - nums[i];
            
            if (map.find(candidate) != map.end()) {
                return {i, map[candidate]};
            } else {
                map[nums[i]] = i;
            }
        }
        
        return {-1, -1};
    }
};
```

## 88. Merge Sorted Array
```cpp
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int write = nums1.size() - 1;
        
        int read1 = m - 1;
        int read2 = n - 1;
        
        while (read1 >= 0 && read2 >= 0) {
            if (nums1[read1] > nums2[read2]) {
                nums1[write] = nums1[read1];
                --read1;
            } else {
                nums1[write] = nums2[read2];
                --read2;
            }
            --write;
        }
        
        while (read1 >= 0) {
            nums1[write] = nums1[read1];
            --read1;
            --write;
        }
        
        while (read2 >= 0) {
            nums1[write] = nums2[read2];
            --read2;
            --write;
        }
    }
};
```

## 476. Number Complement
```cpp
class Solution {
public:
    int findComplement(int num) {
        int complement = 0;
        int pos = 0;
        
        while (num > 0) {
            int digit = num & 1;
            
            complement |= !digit << pos;
            ++pos;
            
            num >>= 1;
        }
        
        return complement;
    }
};
```

## 1009. Complement of Base 10 Integer
```cpp
class Solution {
public:
    int bitwiseComplement(int n) {
        if (n == 0) return 1;
        
        int complement = 0;
        int pos = 0;
        
        while (n > 0) {
            int digit = n & 1;
            
            complement |= !digit << pos;
            ++pos;
            
            n >>= 1;
        }
        
        return complement;
    }
};
```

## 155. Min Stack
```cpp
class MinStack {
public:
    /** initialize your data structure here. */
    MinStack() {

    }
    
    void push(int val) {
        elements.push(val);
        
        if (min.empty() || min.top() > val) {
            min.push(val);
            return;
        }
        
        min.push(min.top());
    }
    
    void pop() {
        elements.pop();
        min.pop();
    }
    
    int top() {
        return elements.top();
    }
    
    int getMin() {
        return min.top();
    }
private:
    stack<int> elements;
    stack<int> min;
};

/**
 * Your MinStack object will be instantiated and called as such:
 * MinStack* obj = new MinStack();
 * obj->push(val);
 * obj->pop();
 * int param_3 = obj->top();
 * int param_4 = obj->getMin();
 */
```

## 1518. Water Bottles
```cpp
class Solution {
public:
    int numWaterBottles(int numBottles, int numExchange) {
        int count = 0;
        
        int bottlesFull = numBottles;
        int bottlesEmpty = 0;
        
        while (bottlesFull) {
            count += bottlesFull;
            bottlesEmpty += bottlesFull;
            
            bottlesFull = bottlesEmpty / numExchange;
            bottlesEmpty = bottlesEmpty % numExchange;
        }
        
        return count;
    }
};
```

## 125. Valid Palindrome
```cpp
class Solution {
public:
    bool isPalindrome(string s) {
        int left = 0;
        int right = s.size() - 1;
        
        while (left < right) {
            if (!isalnum(s[left])) {
                ++left;
                continue;
            }
            
            if (!isalnum(s[right])) {
                --right;
                continue;
            }
            
            if (tolower(s[left]) != tolower(s[right])) {
                return false;
            }
            
            ++left;
            --right;
        }
        
        return true;
    }
};
```

## 242. Valid Anagram
```cpp
class Solution {
public:
    bool isAnagram(string s, string t) {
        if (s.size() != t.size()) {
            return false;
        }
        
        unordered_map<char, int> map;
        
        for (int i = 0; i < s.size(); ++i) {
            ++map[s[i]];
            --map[t[i]];
        }
        
        for (auto it : map) {
            if (it.second != 0) {
                return false;
            }
        }
        
        return true;
    }
};
```

## 121. Best Time to Buy and Sell Stock
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int maxProfit = 0;
        int minPrice = prices[0];
        
        for (int day = 1; day < prices.size(); ++day) {
            minPrice = min(minPrice, prices[day]);
            maxProfit = max(maxProfit, prices[day] - minPrice);
        }
        
        return maxProfit;
    }
};
```

## 136. Single Number
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int unique = 0;
        
        for (int num : nums) {
            unique ^= num;
        }
        
        return unique;
    }
};
```