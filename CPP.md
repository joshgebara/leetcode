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
        ListNode* curr = head;
        while (curr) {
            ListNode* next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
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
        vector<pair<int, string>> rules {{15, "FizzBuzz"}, {3, "Fizz"}, {5, "Buzz"}};
        vector<string> result;

        for (int i = 1; i <= n; i++) {
            string word = "";
            for (const auto [ruleDivisor, ruleWord] : rules) {
                if (i % ruleDivisor == 0) {
                    word += ruleWord;
                    break;
                }
            }
            
            if (word == "") {
                word += to_string(i);
            }

            result.push_back(word);
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
        for (int j = 0; j < nums.size(); ++j) {
            if (nums[j] != 0) {
                swap(nums[i], nums[j]);
                ++i;
            }
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
            if (map.find(c) != map.end()) {
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
        int votes = 1;
        
        for (auto i = 1; i < nums.size(); ++i)
        {
            candidate == nums[i] ? ++votes : --votes;

            if (votes == 0) {
                candidate = nums[i];
                votes = 1;
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
    bool isAnagram(const string& s, const string& t) {
        if (s.size() != t.size()) return false;

        std::array<int, 26> counts{};
        for (auto i = 0; i < s.size(); ++i) {
            const auto indexS = s[i] - 'a';
            const auto indexT = t[i] - 'a';
            ++counts[indexS];
            --counts[indexT];
        }

        return std::all_of(counts.begin(), counts.end(), [](int count) {
            return count == 0;
        });
    }
};
```

### Follow Up
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

## 217. Contains Duplicate
```cpp
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        unordered_set set(nums.begin(), nums.end());
        return nums.size() != set.size();
    }
};
```

## 733. Flood Fill
### Recursive DFS
```cpp
class Solution {
public:
    vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int newColor) {
        if (image[sr][sc] == newColor) {
            return image;
        }
        
        dfs(image, sr, sc, image[sr][sc], newColor);
        return image;
    }
private:
    vector<vector<int>> dirs = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
    
    void dfs(vector<vector<int>>& image, int row, int col, int startColor, int newColor) {
        image[row][col] = newColor;
        
        for (auto dir : dirs) {
            int nextRow = row + dir[0];
            int nextCol = col + dir[1];
            
            if (nextRow < 0 || nextRow >= image.size() || 
                nextCol < 0 || nextCol >= image[0].size() ||
                image[nextRow][nextCol] != startColor) {
                continue;
            }
            
            dfs(image, nextRow, nextCol, startColor, newColor);
        }
    }
};
```

## 303. Range Sum Query - Immutable
```cpp
class NumArray {
private:
    vector<int> prefixSums;

public:
    NumArray(vector<int>& nums) : prefixSums(nums.begin(), nums.end()) {
        for (auto i = 1; i < prefixSums.size(); ++i) {
            prefixSums[i] += prefixSums[i - 1];
        }
    }
    
    int sumRange(int left, int right) {
        if (left == 0) {
            return prefixSums[right];
        }

        return prefixSums[right] - prefixSums[left - 1];
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(left,right);
 */
```

```cpp
class NumArray {
private:
    std::vector<int> prefixSums;

public:
    NumArray(vector<int>& nums) {
        prefixSums.resize(nums.size() + 1, 0);
        std::partial_sum(nums.begin(), nums.end(), prefixSums.begin() + 1);
    }
    
    int sumRange(int left, int right) {
        return prefixSums[right + 1] - prefixSums[left];
    }
};

/**
 * Your NumArray object will be instantiated and called as such:
 * NumArray* obj = new NumArray(nums);
 * int param_1 = obj->sumRange(left,right);
 */
```

## 219. Contains Duplicate II
```cpp
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        unordered_map<int, int> map;
        
        int i = 0;
        for (int j = 0; j < nums.size(); ++j) {
            int num = nums[j];
            if (map.find(num) != map.end() && j - map[num] <= k) {
                return true;
            } else {
                map[num] = j;
            }
        }
        
        return false;
    }
};
```

## 346. Moving Average from Data Stream
```cpp
class MovingAverage {
public:
    /** Initialize your data structure here. */
    MovingAverage(int size) {
        size_ = size;
    }
    
    double next(int val) {
        sum_ += val;
        queue_.push(val);
        
        if (queue_.size() > size_) {
            sum_ -= queue_.front();
            queue_.pop();
        }
        
        return sum_ / queue_.size();
    }
private:
    queue<int> queue_;
    double sum_ = 0;
    int size_;
};

/**
 * Your MovingAverage object will be instantiated and called as such:
 * MovingAverage* obj = new MovingAverage(size);
 * double param_1 = obj->next(val);
 */
```

## 383. Ransom Note
```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        std::array<int, 26> charCounts{};

        for (const auto c : magazine) {
            ++charCounts[c - 'a'];
        }

        for (const auto c : ransomNote) {
            if (charCounts[c - 'a'] == 0) {
                return false;
            }
            --charCounts[c - 'a'];
        }

        return true;
    }
};
```

## 905. Sort Array By Parity
### Two Pass
```cpp
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& nums) {
        vector<int> result;
        
        for (int num : nums) {
            if (num % 2 == 0) {
                result.push_back(num);
            }
        }
        
        for (int num : nums) {
            if (num % 2 != 0) {
                result.push_back(num);
            }
        }
        
        return result;
    }
};
```

### In-Place
```cpp
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& nums) {
        int i = 0;
        for (int j = 0; j < nums.size(); ++j) {
            if (nums[j] % 2 == 0) {
                swap(nums[i], nums[j]);
                ++i;
            }
        }
        
        return nums;
    }
};
```

## 122. Best Time to Buy and Sell Stock II
```cpp
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int profit = 0;
        
        for (int day = 0; day < prices.size() - 1; ++day) {
            profit += max(0, prices[day + 1] - prices[day]);
        }
        
        return profit;
    }
};
```

## 1684. Count the Number of Consistent Strings
```cpp
class Solution {
public:
    int countConsistentStrings(string allowed, vector<string>& words) {
        unordered_set<char> set(begin(allowed), end(allowed));
        int count = words.size();
        
        for (auto word : words) {
            for (char c : word) {
                if (set.find(c) == set.end()) {
                    --count;
                    break;
                }
            }
        }
        
        return count;
    }
};
```

## 392. Is Subsequence
```cpp
class Solution {
public:
    bool isSubsequence(string s, string t) {
        int i = 0;
        
        for (int num : t) {
            if (s[i] == num) {
                ++i;
            }
            
            if (i == s.size()) {
                break;
            }
        }
        
        return i == s.size();
    }
};
```

## 2385. Amount of Time for Binary Tree to Be Infected
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
    int amountOfTime(TreeNode* root, int start) {
        // create adj list
        unordered_map<int, vector<int>> adjList;
        
        queue<pair<TreeNode*, int>> queue;
        queue.push({root, -1});
        
        while (queue.size())
        {
            auto [node, parent] = queue.front();
            queue.pop();
            
            if (parent != -1)
            {
                adjList[node->val].push_back(parent);
                adjList[parent].push_back(node->val);
            }
            
            if (node->left)
            {
                queue.push({node->left, node->val});
            }
            
            if (node->right)
            {
                queue.push({node->right, node->val});
            }
        }
        
        // level order out from start
        int level = -1;
        
        std::queue<int> queue2({start});
        std::set<int> visited({start});
        
        while (queue2.size())
        {
            int size = queue2.size();
            for (int i = 0; i < size; i++) {
                int node = queue2.front();
                queue2.pop();

                for (auto neighbor : adjList[node])
                {
                    if (visited.count(neighbor) == 1)
                    {
                        continue;
                    }

                    visited.insert(neighbor);
                    queue2.push(neighbor);
                } 
	        }
            
            level++;
        }
        
        return level;        
    }
};
```

## 200. Number of Islands
### BFS
```cpp
class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        
        int count = 0;
        for (int row = 0; row < m; row++)
        {
            for (int col = 0; col < n; col++)
            {
                if (grid[row][col] == '1')
                {
                    floodIsland(grid, row, col);
                    count++;
                }
            }
        }
        
        return count;
    }
    
private:
    void floodIsland(vector<vector<char>>& grid, int row, int col)
    {
        int m = grid.size();
        int n = grid[0].size();
        
        vector<pair<int, int>> dirs = {{1, 0}, {0, 1}, {0, -1}, {-1, 0}};
        
        queue<pair<int, int>> queue;
        queue.push({row, col});
        grid[row][col] = '0';
        
        while (queue.size())
        {
            auto [currRow, currCol] = queue.front();
            queue.pop();
            
            for (auto [deltaRow, deltaCol] : dirs)
            {
                auto nextRow = deltaRow + currRow;
                auto nextCol = deltaCol + currCol;
                
                if (nextRow < 0 || nextRow >= m || nextCol < 0 || nextCol >= n)
                {
                    continue;
                }
                
                if (grid[nextRow][nextCol] == '1')
                {
                    grid[nextRow][nextCol] = '0';
                    queue.push({nextRow, nextCol});
                }
            }
        }
    }
};
```

### UnionFind
```cpp
class UnionFind {
private:
    int m_size;
    int m_numOfComponents;
    vector<int> m_parents;
    vector<int> m_sizes;
    
public:
    UnionFind(int size) {
        m_size = size;
        m_numOfComponents = size;
        
        for (int i = 0; i < size; i++)
        {
            m_parents.push_back(i);
            m_sizes.push_back(1);
        }
    }
    
    int numOfComponents() const { return m_numOfComponents; }
    
    void unionComponents(int a, int b)
    {
        auto parentA = find(a);
        auto parentB = find(b);
        
        if (parentA == parentB)
        {
            return;
        }
        
        if (m_sizes[parentA] < m_sizes[parentB])
        {
            m_sizes[parentB] += m_sizes[parentA];
            m_parents[parentA] = parentB;
        }
        else
        {
            m_sizes[parentA] += m_sizes[parentB];
            m_parents[parentB] = parentA;
        }
        
        m_numOfComponents--;
    }
    
    int find(int a)
    {
        auto root = a;
        while (root != m_parents[root])
        {
            root = m_parents[root];
        }
        
        while (a != root)
        {
            auto next = m_parents[a];
            m_parents[a] = root;
            a = next;
        }
        
        return root;
    }
};

class Solution {
public:
    int numIslands(vector<vector<char>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        
        vector<pair<int, int>> dirs = {{1, 0}, {0, 1}, {0, -1}, {-1, 0}};
        
        UnionFind unionFind(m * n);
        int waterCount = 0;
        for (int row = 0; row < m; row++)
        {
            for (int col = 0; col < n; col++)
            {
                if (grid[row][col] == '1')
                {
                    for (auto [deltaRow, deltaCol] : dirs)
                    {
                        auto nextRow = deltaRow + row;
                        auto nextCol = deltaCol + col;

                        if (nextRow < 0 || nextRow >= m || nextCol < 0 || nextCol >= n)
                        {
                            continue;
                        }

                        if (grid[nextRow][nextCol] == '1')
                        {
                            unionFind.unionComponents(row * n + col, nextRow * n + nextCol);
                        }
                    }
                }
                else 
                {
                    waterCount++;
                }
            }
        }
        
        return unionFind.numOfComponents() - waterCount;
    }
};
```

## 1443. Minimum Time to Collect All Apples in a Tree
```cpp
class Solution {
public:
    int minTime(int n, vector<vector<int>>& edges, vector<bool>& hasApple) {
        // create adjList
        unordered_map<int, vector<int>> graph;
        for (auto edge : edges)
        {
            auto a = edge[0];
            auto b = edge[1];
            
            graph[a].push_back(b);
            graph[b].push_back(a);
        }
        
        // dfs graph
        set<int> visited;
        auto [gotApple, count] = dfs(graph, 0, hasApple, visited);
        
        if (gotApple)
        {
            return count - 2;
        }
        
        return count;
    }
    
    pair<bool, int> dfs(unordered_map<int, 
                        vector<int>>& graph, 
                        int node, vector<bool>& hasApple, 
                        set<int>& visited)
    {
        if (visited.count(node) == 1)
        {
            return {false, 0};
        }
        
        visited.insert(node);
        
        int time = 0;
        for (auto neighbor : graph[node])
        {
            auto [gotApple, steps] = dfs(graph, neighbor, hasApple, visited);
            if (gotApple)
            {
                time += steps;
            }
        }
        
        if (time > 0)
        {
            return {true, time + 2};
        }
        else if (hasApple[node])
        {
            return {true, 2};
        }
        else
        {
            return {false, 0};
        }
    }
};
```

## 752. Open the Lock
```cpp
class Solution {
public:
    int openLock(vector<string>& deadends, string target) {
        set<string> visited(deadends.begin(), deadends.end());
        
        if (visited.count("0000") == 1)
        {
            return -1;
        }
        
        visited.insert("0000");
        
        queue<string> queue;
        queue.push("0000");
        
        int steps = 0;
        while (queue.size())
        {
            int size = queue.size();
            for (auto i = 0; i < size; i++)
            {
                auto node = queue.front();
                queue.pop();

                if (node == target)
                {
                    return steps;
                }

                // get neighbors
                for (auto neighbor : getNeighbors(node))
                {
                    if (visited.count(neighbor) == 0)
                    {
                        visited.insert(neighbor);
                        queue.push(neighbor);
                    }
                }
            }
            steps++;
        }
        
        return -1;
    }
    
    vector<string> getNeighbors(string node)
    {
        vector<string> neighbors;
        
        for (int wheel = 0; wheel < node.size(); wheel++)
        {
            auto code = node;
            auto digit = code[wheel];
            
            // rotate left
            code[wheel] = (digit - '0' - 1 + 10) % 10 + '0';
            neighbors.push_back(code);
            
            // rotate right
            code[wheel] = (digit - '0' + 1) % 10 + '0';
            neighbors.push_back(code);
        }
        
        return neighbors;
    }
};
```

## 103. Binary Tree Zigzag Level Order Traversal
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
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if (!root)
        {
            return {};
        }
        
        queue<TreeNode*> queue;
        queue.push(root);
        
        vector<vector<int>> levels;
        int shouldReverse = 0;
        
        while (queue.size())
        {
            int size = queue.size();
            vector<int> level(size);
            
            for (int i = 0; i < size; i++)
            {
                auto node = queue.front();
                queue.pop();
                
                if (shouldReverse & 1)
                {
                    level[size - i - 1] = node->val;
                }
                else
                {
                    level[i] = node->val;
                }
                
                if (node->left)
                {
                    queue.push(node->left);
                }
                
                if (node->right)
                {
                    queue.push(node->right);
                }
            }
            
            shouldReverse ^= 1;
            levels.push_back(level);
        }
        
        return levels;
    }
};
```

## 802. Find Eventual Safe States
```cpp
class Solution {
public:
    vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
        int n = graph.size();
        vector<int> result;
        
        unordered_map<int, bool> isSafe;
        vector<bool> visited(n, false);
        
        for (int node = 0; node < n; node++)
        {
            if (isSafe.count(node) == 1)
            {
                if (isSafe[node])
                {
                    result.push_back(node);
                }
                continue;
            }
                        
            if (dfs(node, isSafe, graph, visited))
            {
                result.push_back(node);
            }
        }
        
        return result;
    }
    
    bool dfs(int node, unordered_map<int, bool>& isSafe, vector<vector<int>>& graph, vector<bool>& visited)
    {        
        // is terminal
        if (graph[node].size() == 0)
        {
            isSafe[node] = true;
            return true;
        }
        
        if (isSafe.count(node) == 1)
        {
            return isSafe[node];
        }
        
        // cycle
        if (visited[node])
        {
            return false;
        }
        
        visited[node] = true;
        
        for (auto neighbor : graph[node])
        {
            if (!dfs(neighbor, isSafe, graph, visited))
            {
                isSafe[node] = false;
                return false;
            }
        }
        
        visited[node] = false;
        
        isSafe[node] = true;
        return true;
    }
};
```

## 1625. Lexicographically Smallest String After Applying Operations
```cpp
class Solution {
public:
    string findLexSmallestString(string s, int a, int b) {
        queue<string> queue;
        queue.push(s);
        
        set<string> visited;
        visited.insert(s);
        
        auto minString = s;
        
        while (queue.size())
        {
            auto node = queue.front();
            queue.pop();
            
            if (minString > node)
            {
                minString = node;
            }
            
            auto addedString = add(node, a);
            if (visited.count(addedString) == 0)
            {
                visited.insert(addedString);
                queue.push(addedString);
            }
            
            auto rotatedString = rotate(node, b);
            if (visited.count(rotatedString) == 0)
            {
                visited.insert(rotatedString);
                queue.push(rotatedString);
            }
        }
        
        return minString;
    }
    
private:
    string add(const string& s, int a) 
    {
        stringstream ss;
        
        for (auto i = 0; i < s.size(); i++)
        {
            if (i & 1)
            {
                ss << (s[i] - '0' + a) % 10;
            }
            else
            {
                ss << s[i];
            }
        }
        
        return ss.str();
    }
    
    string rotate(string s, int b)
    {
        rotate(s, 0, s.size());
        rotate(s, 0, b);
        rotate(s, b, s.size());
        return s;
    }
    
    void rotate(string& s, int left, int right)
    {
        while (left < right)
        {
            auto temp = s[left];
            s[left] = s[right];
            s[right] = temp;
            
            left++;
            right--;
        }
    }
};
```

## 1254. Number of Closed Islands
```cpp
class Solution {
    int dfs(vector<vector<int>>& grid, int row, int col)
    {
        vector<vector<int>> dirs = {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
        
        int rowLen = grid.size();
        int colLen = grid[0].size();
        
        // if out of bounds return 0
        if (row < 0 || row >= rowLen || col < 0 || col >= colLen)
        {
            return 0;
        }
        
        // if value is 1 return 1
        if (grid[row][col] == 1)
        {
            return 1;
        }
        
        // set cell to 1 once visited
        grid[row][col] = 1;
        
        // get neighbors if any are 0 then return 0
        int result = 1;
        for (auto delta : dirs)
        {
            auto nextRow = row + delta[0];
            auto nextCol = col + delta[1];
            
            if (dfs(grid, nextRow, nextCol) == 0)
            {
                result = 0;
            }
        }
        
        return result;
    }
    
public:
    int closedIsland(vector<vector<int>>& grid) {
        int rowLen = grid.size();
        int colLen = grid[0].size();
        
        int numOfClosedIslands{0};
        for (auto row = 1; row < rowLen - 1; row++)
        {
            for (auto col = 1; col < colLen - 1; col++)
            {
                // Found land
                if (grid[row][col] == 0)
                {
                    numOfClosedIslands += dfs(grid, row, col);
                }
            }
        }
        
        return numOfClosedIslands;
    }
};
```

## 1765. Map of Highest Peak
```cpp
class Solution {
public:
    vector<vector<int>> highestPeak(vector<vector<int>>& isWater) {
        auto rowLen = isWater.size();
        auto colLen = isWater[0].size();
        
        queue<pair<int, int>> queue;
        
        // collect water cells into a queue
        for (auto row = 0; row < rowLen; row++)
        {
            for (auto col = 0; col < colLen; col++)
            {
                if (isWater[row][col] == 1)
                {
                    queue.push({row, col});
                    isWater[row][col] = 0;
                }
                else
                {
                    isWater[row][col] = -1;
                }
            }
        }
        
        // process cells, mark neighbors heights
        vector<pair<int, int>> dirs = {{1, 0}, {0, 1}, {0, -1}, {-1, 0}};
        auto level = 1;
        
        while (queue.size())
        {
            auto size = queue.size();
            for (auto i = 0; i < size; i++)
            {
                auto [row, col] = queue.front();
                queue.pop();
                
                // get neighbors
                for (auto [deltaRow, deltaCol] : dirs)
                {
                    auto nextRow = row + deltaRow;
                    auto nextCol = col + deltaCol;
                    
                    if (nextRow < 0 || nextRow >= isWater.size() || 
                        nextCol < 0 || nextCol >= isWater[0].size())
                    {
                        continue;
                    }
                    
                    if (isWater[nextRow][nextCol] == -1)
                    {
                        isWater[nextRow][nextCol] = level;
                        queue.push({nextRow, nextCol});
                    }
                }
            }
            
            level++;
        }
        
        return isWater;
    }
};
```

## 2368. Reachable Nodes With Restrictions
```cpp
class Solution {
public:
    int reachableNodes(int n, vector<vector<int>>& edges, vector<int>& restricted) {
        // create adjacency list
        unordered_map<int, vector<int>> graph;
        for (auto edge : edges)
        {
            auto a = edge[0];
            auto b = edge[1];
            
            if (!graph.count(a)) graph[a] = {};
            if (!graph.count(b)) graph[b] = {};
            
            graph[a].push_back(b);
            graph[b].push_back(a);
        }
        
        // add restricted values to an unordered_set
        unordered_set set(restricted.begin(), restricted.end());
        set.insert(0);
        
        // bfs from 0
        queue<int> queue;
        queue.push(0);
        
        int nodes = 0;
        while (queue.size())
        {
            auto node = queue.front();
            queue.pop();
            
            nodes++;
            
            for (auto neighbor : graph[node])
            {
                // if neighbor in restricted skip
                if (set.count(neighbor) == 1) continue;
                set.insert(neighbor);
                
                queue.push(neighbor);
            }
        }
        
        return nodes;
    }
};
```

## 967. Numbers With Same Consecutive Differences
```cpp
class Solution {
public:
    vector<int> numsSameConsecDiff(int n, int k) {
        deque<int> q;
        for (auto digit = 1; digit <= 9; digit++)
        {
            q.push_back(digit);
        }
        
        for (auto level = 1; level < n; level++)
        {
            auto size = q.size();
            for (auto i = 0; i < size; i++)
            {
                auto node = q.front();
                q.pop_front();

                // go left if digit - k >= 0
                auto leftDigit = (node % 10) - k;
                auto leftNum = node * 10 + leftDigit;
                if (leftDigit >= 0)
                {
                    q.push_back(leftNum);
                }
                
                // go right if digit + k < 10
                auto rightDigit = (node % 10) + k;
                auto rightNum = node * 10 + rightDigit;
                // leftDigit != rightDigit to avoid duplicates
                // Ex: n = 2, k = 0
                if (leftDigit != rightDigit && rightDigit < 10)
                {
                    q.push_back(rightNum);
                }
            }
        }
    
        vector<int> result(q.begin(), q.end());
        return result;
    }
};
```

## 1466. Reorder Routes to Make All Paths Lead to the City Zero
```cpp
class Solution {
    using graph = unordered_map<int, vector<pair<int, int>>>;
    
    int numOfFlips(graph& bi_graph)
    {
        int flips = 0;
        deque<pair<int, int>> queue{{0, 0}};
        unordered_set<int> visited{0};
        
        while (queue.size())
        {
            auto [node, direction] = queue.front();
            queue.pop_front();
            
            for (auto [edge, edgeDirection] : bi_graph[node])
            {
                if (visited.count(edge) == 0)
                {
                    flips += edgeDirection;

                    visited.insert(edge);
                    queue.push_back({edge, edgeDirection});
                }
            }
        }
        return flips;
    }
    
public:
    int minReorder(int n, vector<vector<int>>& connections) {
        graph bi_graph;
        
        for (auto& connection : connections)
        {        
            bi_graph[connection[0]].push_back({connection[1], 1});
            bi_graph[connection[1]].push_back({connection[0], 0});
        }
        
        return numOfFlips(bi_graph);
    }
};
```

## 1202. Smallest String With Swaps
```cpp
class UnionFind {
public:
    int size;
    vector<int> parents;
    vector<int> sizes;
    
    UnionFind(int size) : size{size} {
        for (auto i = 0; i < size; i++)
        {
            this->parents.push_back(i);
            this->sizes.push_back(1);
        }
    }
    
    void unionElements(int a, int b) {
        auto parentA = this->find(a);
        auto parentB = this->find(b);
        
        if (parentA == parentB)
        {
            return;
        }
        
        if (this->sizes[parentA] < this->sizes[parentB])
        {
            this->sizes[parentB] += this->sizes[parentA];
            this->parents[parentA] = parentB;
        }
        else
        {
            this->sizes[parentA] += this->sizes[parentB];
            this->parents[parentB] = parentA;
        }
    }
    
    int find(int a) {
        auto root = a;
        while (root != this->parents[root])
        {
            root = this->parents[root];
        }
        
        while (root != a)
        {
            auto next = this->parents[a];
            this->parents[a] = root;
            a = next;
        }
        
        return root;
    }
};

class Solution {
public:
    string smallestStringWithSwaps(string s, vector<vector<int>>& pairs) {
        UnionFind unionFind(s.size());
        
        for (auto& pair : pairs)
        {
            unionFind.unionElements(pair[0], pair[1]);
        }
        
        // collect chars for each component
        unordered_map<int, vector<int>> m;
        for (auto i = 0; i < s.size(); i++)
        {
            auto parent = unionFind.find(i);
            m[parent].push_back(i);
        }
        
        // sort chars in each component
        for (auto& [key, value] : m)
        {
            sort(value.begin(), value.end(), [&] (int a, int b) {
                return s[a] > s[b];
            });
        }
        
        // construct final string
        auto result = s;
        for (auto i = 0; i < result.size(); i++)
        {
            auto parent = unionFind.find(i);
            int index = m[parent].back();
            m[parent].pop_back();
            result[i] = s[index];
        }
        
        return result;
    }
};
```

## 623. Add One Row to Tree
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
    TreeNode* addOneRow(TreeNode* root, int val, int depth) {
        if (depth == 0)
        {
            return root;
        }
        
        /*
        If depth == 1 that means there is no depth depth - 1 at all, 
        then create a tree node with value val as the new root of the 
        whole original tree, and the original tree is the new root's left subtree.
        */
        if (depth == 1)
        {
            auto newRoot{new TreeNode{val}};
            newRoot->left = root;
            return newRoot;
        }
        
        deque<TreeNode*> queue{root};
        int level = 1;
        while (queue.size())
        {
            auto size = queue.size();
            for (auto i = 0; i < size; i++)
            {
                auto cur = queue.front();
                queue.pop_front();
                
                /*
                Given the integer depth, for each not null tree node cur at the depth depth - 1,
                create two tree nodes with value val as cur's left subtree root and right subtree
                root.        
                */
                if (level == depth - 1)
                {
                    auto originalLeft = cur->left;
                    auto originalRight = cur->right;
                    
                    /*
                    cur's original left subtree should be the left subtree 
                    of the new left subtree root.
                    */
                    cur->left = new TreeNode{val};
                    cur->left->left = originalLeft;
                    
                    /*
                    cur's original right subtree should be the right 
                    subtree of the new right subtree root.
                    */
                    cur->right = new TreeNode{val};
                    cur->right->right = originalRight;
                }
                
                if (cur->left)
                {
                    queue.push_back(cur->left);
                }
                
                if (cur->right)
                {
                    queue.push_back(cur->right);
                }
            }
            
            if (level == depth - 1)
            {
                return root;
            }
            
            level++;
            
        }
        return root;
    }
};
```

## 1920. Build Array from Permutation
```cpp
class Solution {
public:
    vector<int> buildArray(vector<int>& nums) {
        constexpr unsigned int digitUpperBound = 10;
        constexpr unsigned int mask = (1 << digitUpperBound) - 1;
        
        for (auto i = 0; i < nums.size(); i++)
        {
            // get original number
            auto originalNum = nums[nums[i]] & mask;
            
            // set result number
            nums[i] |= (originalNum << digitUpperBound);
        }
        
        // remove original numbers
        for (auto i = 0; i < nums.size(); i++)
        {
            nums[i] >>= digitUpperBound;
        }
        
        return nums;
    }
};
```

## 1929. Concatenation of Array
```cpp
class Solution {
public:
    vector<int> getConcatenation(vector<int>& nums) {
        const auto n = nums.size();
        vector<int> result(2 * n);
        
        for (auto i = 0; i < n; i++)
        {
            result[i] = nums[i];
            result[i + n] = nums[i];
        }
        
        return result;
    }
};
```

## 2469. Convert the Temperature
```cpp
class Solution {
public:
    vector<double> convertTemperature(double celsius) {
        return {celsius + 273.15, celsius * 1.80 + 32.00};
    }
};
```

## 1689. Partitioning Into Minimum Number Of Deci-Binary Numbers
```cpp
class Solution {
public:
    int minPartitions(string n) {
        return *max_element(n.begin(), n.end()) - '0';
    }
};
```

## 1108. Defanging an IP Address
```cpp
class Solution {
public:
    string defangIPaddr(string address) {
        string result;
        
        for (auto& c : address)
        {
            if (c == '.')
            {
                result += "[.]";
            }
            else
            {
                result += c;
            }
        }
        
        
        return result;
    }
};
```

## 1480. Running Sum of 1d Array
```cpp
class Solution {
public:
    vector<int> runningSum(vector<int>& nums) {
        for (auto i = 1; i < nums.size(); ++i) {
            nums[i] += nums[i - 1];
        }

        return nums;
    }
};
```

```cpp
class Solution {
public:
    vector<int> runningSum(vector<int>& nums) {
        std::partial_sum(nums.begin(), nums.end(), nums.begin());
        return nums;
    }
};
```

## 1470. Shuffle the Array
```cpp
class Solution {
public:
    vector<int> shuffle(vector<int>& nums, int n) {
        constexpr int digitUpperBound {10};
        constexpr int mask {(1 << digitUpperBound) - 1};
        
        for (int i {0}; i < nums.size() / 2; i++)
        {
            int j{i + n};
            
            nums[i] <<= digitUpperBound;
            nums[i] |= nums[j];
        }
        
        int j = (nums.size() / 2) - 1;
        for (int i = nums.size() - 1; i >= 0; i -= 2)
        {
            auto x = nums[j] >> digitUpperBound;
            auto y = nums[j] & mask;
            
            nums[i - 1] = x;
            nums[i] = y;
            
            j--;
        }
        
        return nums;
    }
};
```

## 1512. Number of Good Pairs
```cpp
class Solution {
public:
    int numIdenticalPairs(vector<int>& nums) {
        vector<int> buckets(101, 0);
        
        int result = 0;
        for (int i = 0; i < nums.size(); i++)
        {
            buckets[nums[i]]++;
        }
        
        for (auto& n : buckets)
        {
            result += n * (n - 1) / 2;
        }
        
        return result;
    }
};
```

## 1672. Richest Customer Wealth
```cpp
class Solution {
public:
    int maximumWealth(vector<vector<int>>& accounts) {
        int result{0};
        
        for (auto& account : accounts)
        {
            result = max(result, std::accumulate(account.begin(), account.end(), 0));
        }
        
        return result;
    }
};
```

## 771. Jewels and Stones
```cpp
class Solution {
public:
    int numJewelsInStones(string jewels, string stones) {
        int count{0};
        unordered_set<char> j(jewels.begin(), jewels.end());
        
        for (const auto& c : stones)
        {
            count += j.count(c);
        }
        
        return count;
    }
};
```

## 1603. Design Parking System
```cpp
class ParkingSystem {
    vector<int> spaces;
    
public:
    
    ParkingSystem(int big, int medium, int small) : spaces{big, medium, small} {
    }
    
    bool addCar(int carType) {
        if (spaces[carType - 1] > 0)
        {
            spaces[carType - 1]--;
            return true;
        }
        
        return false;
    }
};

/**
 * Your ParkingSystem object will be instantiated and called as such:
 * ParkingSystem* obj = new ParkingSystem(big, medium, small);
 * bool param_1 = obj->addCar(carType);
 */
```

## 2114. Maximum Number of Words Found in Sentences
```cpp
class Solution {
    int getNumOfWords(const string& sentence)
    {
        if (sentence.size() == 0)
        {
            return 0;
        }
        
        int numOfWords{1};
        
        for (const auto& c : sentence)
        {
            if (c == ' ')
            {
                numOfWords++;
            }
        }
        
        return numOfWords;
    }
    
public:
    int mostWordsFound(vector<string>& sentences) {
        int maxWords{0};
        
        for (const auto& sentence : sentences)
        {
            int numOfWords = getNumOfWords(sentence);
            maxWords = max(maxWords, numOfWords);
        }
        
        return maxWords;
    }
};
```

## 2160. Minimum Sum of Four Digit Number After Splitting Digits
```cpp
class Solution {
public:
    int minimumSum(int num) {
        vector<int> digits;
        
        int n{num};
        while (n)
        {
            const int digit {n % 10};
            digits.push_back(digit);
            n /= 10;
        }
        
        sort(digits.begin(), digits.end());
        
        const int num1 = digits[0] * 10 + digits[3];
        const int num2 = digits[1] * 10 + digits[2];
        
        return num1 + num2;
    }
};
```

## 2413. Smallest Even Multiple
```cpp
class Solution {
public:
    int smallestEvenMultiple(int n) {
        return n << (n & 1);
    }
};
```

## 1431. Kids With the Greatest Number of Candies
```cpp
class Solution {
public:
    vector<bool> kidsWithCandies(vector<int>& candies, int extraCandies) {
        int maxNum{0};
        
        for (auto c : candies)
        {
            maxNum = max(maxNum, c);
        }
        
        vector<bool> result(candies.size());
        for (int i = 0; i < candies.size(); i++)
        {
            result[i] = maxNum <= candies[i] + extraCandies;
        }
        
        return result;
    }
};
```

## 2181. Merge Nodes in Between Zeros
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
    ListNode* mergeNodes(ListNode* head) {
        ListNode* prev = nullptr;
        auto curr = head;
        
        while (curr->next)
        {
            auto runner = curr->next;
            while (runner->val != 0)
            {
                curr->val += runner->val;
        
                auto next = runner->next;
                delete runner;
                curr->next = next;
                runner = next;
            }
            
            prev = curr;
            curr = curr->next;
        }
        
        prev->next = nullptr;
        return head;
    }
};
```

## 2236. Root Equals Sum of Children
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
    bool checkTree(TreeNode* root) {
        return root->val == root->left->val + root->right->val;
    }
};
```

## 1281. Subtract the Product and Sum of Digits of an Integer
```cpp
class Solution {
public:
    int subtractProductAndSum(int n) {
        int product = 1;
        int sum = 0;
        
        while (n)
        {
            int digit = n % 10;
            product *= digit;
            sum += digit;
            n /= 10;
        }
        
        return product - sum;
    }
};
```

## 1365. How Many Numbers Are Smaller Than the Current Number
```cpp
class Solution {
public:
    vector<int> smallerNumbersThanCurrent(vector<int>& nums) {
        vector<int> counts(101, 0);
        
        for (auto& num : nums)
        {
            counts[num]++;
        }
        
        int rollingCount{0};
        for (int i = 0; i < counts.size(); i++)
        {
            auto tmp = counts[i];
            counts[i] = rollingCount;
            rollingCount += tmp;
        }
        
        vector<int> result;
        for (auto& num : nums)
        {
            result.push_back(counts[num]);
        }
        
        return result;
    }
};
```

## 1678. Goal Parser Interpretation
```cpp
class Solution {
public:
    string interpret(string command) {
        string result = "";

        for (int i = 0; i < command.size();)
        {
            if (command[i] == 'G')
            {
                result += 'G';
                i++;
            }
            else if (command[i] == '(' && command[i + 1] == ')')
            {
                result += 'o';
                i += 2;
            }
            else
            {
                result += "al";
                i += 4;
            }
        }

        return result;
    }
};
```

## 1720. Decode XORed Array
```cpp
class Solution {
public:
    vector<int> decode(vector<int>& encoded, int first) {
        vector<int> result{first};

        for (auto& e : encoded)
        {
            result.push_back(result.back() ^ e);
        }

        return result;
    }
};
```

## 807. Max Increase to Keep City Skyline
```cpp
class Solution {
public:
    int maxIncreaseKeepingSkyline(vector<vector<int>>& grid) {
        vector<int> maxRowHeights(grid.size());
        vector<int> maxColHeights(grid[0].size());

        for (int row = 0; row < grid.size(); row++)
        {
            for (int col = 0; col < grid[0].size(); col++)
            {
                maxRowHeights[row] = max(maxRowHeights[row], grid[row][col]);
                maxColHeights[col] = max(maxColHeights[col], grid[row][col]);
            }
        }

        int totalSum{0};
        for (auto row = 0; row < grid.size(); row++)
        {
            for (auto col = 0; col < grid[0].size(); col++)
            {
                int diff = min(maxRowHeights[row], maxColHeights[col]) - grid[row][col];
                totalSum += diff;
            }
        }
        return totalSum;
    }
};
```

## 1313. Decompress Run-Length Encoded List
```cpp
class Solution {
public:
    vector<int> decompressRLElist(vector<int>& nums) {
        vector<int> result;

        for (auto i = 0; i < nums.size(); i += 2)
        {
            result.insert(result.end(), nums[i], nums[i + 1]);
        }

        return result;
    }
};
```

## 2433. Find The Original Array of Prefix Xor
```cpp
class Solution {
public:
    vector<int> findArray(vector<int>& pref) {
        vector<int> result;

        for (int i = 0; i < pref.size(); i++)
        {
            if (i == 0)
            {
                result.push_back(pref[i]);
                continue;
            }
            
            result.push_back(pref[i] ^ pref[i - 1]);
        }
        
        return result;
    }
};
```

## 2265. Count Nodes Equal to Average of Subtree
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
    array<int, 3> dfs(TreeNode* root)
    {
        if (!root)
        {
            return {0,0,0};
        }

        auto [leftSum, leftSize, leftCount] = dfs(root->left);
        auto [rightSum, rightSize, rightCount] = dfs(root->right);

        auto totalSum = leftSum + rightSum + root->val;
        auto totalSize = leftSize + rightSize + 1;
        auto totalCount = leftCount + rightCount;

        int avg = totalSum / totalSize;
        return {totalSum, totalSize, totalCount + (root->val == avg)};
    }

public:
    int averageOfSubtree(TreeNode* root) {
        auto result = dfs(root);
        return result[2];
    }
};
```

## 1282. Group the People Given the Group Size They Belong To
```cpp
class Solution {
public:
    vector<vector<int>> groupThePeople(vector<int>& groupSizes) {
        vector<vector<int>> result;
        vector<vector<int>> groups(groupSizes.size() + 1);

        for (int i = 0; i < groupSizes.size(); i++)
        {
            groups[groupSizes[i]].push_back(i);

            if (groups[groupSizes[i]].size() == groupSizes[i])
            {
                result.push_back(groups[groupSizes[i]]);
                groups[groupSizes[i]] = {};
            }
        }

        return result;
    }
};
```

## 2194. Cells in a Range on an Excel Sheet
```cpp
class Solution {
public:
    vector<string> cellsInRange(string s) {
        char col1{s[0]};
        char row1{s[1]};

        char col2{s[3]};
        char row2{s[4]};

        vector<string> result;
        for (char col = col1; col <= col2; col++)
        {
            for (char row = row1; row <= row2; row++)
            {
                result.push_back({col, row});
            }
        }

        return result;
    }
};
```

## 1656. Design an Ordered Stream
```cpp
class OrderedStream {
    unordered_map<int, string> m;
    int nextId{1};
public:
    OrderedStream(int n) {
        
    }
    
    vector<string> insert(int idKey, string value) {
        m[idKey] = value;

        if (idKey != nextId) 
            return {};

        vector<string> result;
        while (m.count(idKey))
        {
            result.push_back(m[idKey]);
            m.erase(idKey);
            idKey++;
        }

        nextId = idKey;
        return result;
    }
};

/**
 * Your OrderedStream object will be instantiated and called as such:
 * OrderedStream* obj = new OrderedStream(n);
 * vector<string> param_1 = obj->insert(idKey,value);
 */
```

## 1038. Binary Search Tree to Greater Sum Tree
### Recursive
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
    int sum{0};
public:
    TreeNode* bstToGst(TreeNode* root) {
        if (!root) return root;
        
        bstToGst(root->right);
        
        sum += root->val;
        root->val = sum;

        bstToGst(root->left);
        
        return root;
    }
};
```

### Iterative
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
    int sum{0};

public:
    TreeNode* bstToGst(TreeNode* root) {
        vector<TreeNode*> stack;
        auto curr = root;

        while (curr || stack.size())
        {
            while (curr)
            {
                stack.push_back(curr);
                curr = curr->right;
            }

            auto node = stack.back();
            stack.pop_back();

            sum += node->val;
            node->val = sum;

            curr = node->left;
        }

        return root;  
    }
};
```

## 2325. Decode the Message
```cpp
class Solution {
public:
    string decodeMessage(string key, string message) {
        array<char, 128> cipher{0};
        char alpha{'a'};

        for (auto& k : key)
        {
            if (isalpha(k) && cipher[k] == 0)
            {
                cipher[k] = alpha;
                alpha++;
            }
        }

        string result;
        for (auto& c : message)
        {
            if (cipher[c] == 0)
                result += c;
            else
                result += cipher[c];
        }

        return result;
    }
};
```

## 1221. Split a String in Balanced Strings
```cpp
class Solution {
public:
    int balancedStringSplit(string s) {
        int count{0};
        int balance{0};
        for (auto& c : s)
        {
            balance += (c == 'R') ? -1 : 1;

            if (balance == 0)
                count++;
        }

        return count;
    }
};
```

## 1637. Widest Vertical Area Between Two Points Containing No Points
```cpp
class Solution {
public:
    int maxWidthOfVerticalArea(vector<vector<int>>& points) {
        set<int> s;
        for (const auto& p : points)
        {
            s.insert(p[0]);
        }

        int maxArea{0};
        for (auto i = next(begin(s)); i != end(s); i++)
        {
            maxArea = max(maxArea, *i - *prev(i));
        }

        return maxArea;
    }
};
```

## 1486. XOR Operation in an Array
```cpp
class Solution {
public:
    int xorOperation(int n, int start) {
        int result{0};
        for (int i = 0; i < n; i++)
        {
            result ^= start + 2 * i;
        }
        return result;
    }
};
```

## 1832. Check if the Sentence Is Pangram
```cpp
class Solution {
public:
    bool checkIfPangram(string sentence) {
        int pangramMask = (1 << 26) - 1;
        int mask = 0;

        for (const auto& c : sentence)
        {
            mask |= 1 << (c - 'a');
        }

        return pangramMask == mask;
    }
};
```

## 2367. Number of Arithmetic Triplets
```cpp
class Solution {
public:
    int arithmeticTriplets(vector<int>& nums, int diff) {
        unordered_map<int, int> m;
        for (const auto& num : nums)
        {
            m[num]++;
        }

        int count{0};
        for (const auto& num : nums)
        {
            auto jCount = m.count(num + diff);
            auto kCount = m.count(num + diff + diff);
            if (jCount && kCount)
            {
                count += jCount * kCount;
            }
        }

        return count;
    }
};
```

## 1769. Minimum Number of Operations to Move All Balls to Each Box
```cpp
class Solution {
public:
    vector<int> minOperations(string boxes) {
        vector<int> result(boxes.size(), 0);

        int sumOfIndices{0};
        int countOfOnes{0};
        for (int i = 0; i < boxes.size(); i++)
        {
            int moves = abs(i * countOfOnes - sumOfIndices);
            result[i] += moves;
            
            if (boxes[i] == '1')
            {
                sumOfIndices += i;
                countOfOnes++;
            }
        }

        sumOfIndices = 0;
        countOfOnes = 0;
        for (int i = boxes.size() - 1; i >= 0; i--)
        {
            int moves = abs(i * countOfOnes - sumOfIndices);
            result[i] += moves;
            
            if (boxes[i] == '1')
            {
                sumOfIndices += i;
                countOfOnes++;
            }
        }
        
        return result;
    }
};
```

## 1329. Sort the Matrix Diagonally
```cpp
class Solution {
public:
    vector<vector<int>> diagonalSort(vector<vector<int>>& mat) {
        unordered_map<int, priority_queue<int,vector<int>,greater<int>>> m;

        for (int row{0}; row < mat.size(); row++)
        {
            for (int col{0}; col < mat[0].size(); col++)
            {
                m[row - col].push(mat[row][col]);
            }
        }

        for (int row{0}; row < mat.size(); row++)
        {
            for (int col{0}; col < mat[0].size(); col++)
            {
                mat[row][col] = m[row - col].top();
                m[row - col].pop();
            }
        }

        return mat;
    }
};
```

## 1662. Check If Two String Arrays are Equivalent
```cpp
class Solution {
public:
    bool arrayStringsAreEqual(vector<string>& word1, vector<string>& word2) {
        int word1Index{0};
        int word2Index{0};

        int subString1Index{0};
        int subString2Index{0};

        while (word1Index < word1.size() && word2Index < word2.size())
        {
            auto subString1{word1[word1Index]};
            auto subString2{word2[word2Index]};

            if (subString1[subString1Index] != subString2[subString2Index])
            {
                return false;
            }

            subString1Index++;
            subString2Index++;

            // move to next substring if needed
            if (subString1Index >= subString1.size())
            {
                word1Index++;
                subString1Index = 0;
            }

            // move to next substring if needed
            if (subString2Index >= subString2.size())
            {
                word2Index++;
                subString2Index = 0;
            }
        }

        return word1Index >= word1.size() && word2Index >= word2.size();
    }
};
```

## 804. Unique Morse Code Words
```cpp
class Solution {
    array<string, 26> codeMap{".-","-...","-.-.","-..",".","..-.","--.",
    "....","..",".---","-.-",".-..","--","-.","---",".--.","--.-",".-.","...","-",
    "..-","...-",".--","-..-","-.--","--.."};

    string toMorse(const string& word) const
    {
        string morseCode;
        for (auto& c : word)
        {
            morseCode += codeMap[c - 'a'];
        }

        return morseCode;
    }

public:
    int uniqueMorseRepresentations(vector<string>& words) {
        unordered_set<string> s;

        for (const auto& w : words)
        {
            s.insert(toMorse(w));
        }

        return s.size();
    }
};
```

## 1614. Maximum Nesting Depth of the Parentheses
```cpp
class Solution {
public:
    int maxDepth(string s) {
        int nestingDepth{0};
        int balance{0};
        
        for (const auto& c : s)
        {
            if (c == '(')
            {
                balance++;
                nestingDepth = max(nestingDepth, balance);
            }
            else if (c == ')')
            {
                balance--;
            }
        }

        return nestingDepth;
    }
};
```

## 2125. Number of Laser Beams in a Bank
```cpp
class Solution {
public:
    int numberOfBeams(vector<string>& bank) {
        int total{0};
        int prevRowNum{0};
        for (const auto& b : bank)
        {
            auto currRowNum { count(b.begin(), b.end(), '1') };
            if (currRowNum)
            {
                total += prevRowNum * currRowNum;
                prevRowNum = currRowNum;
            }
        }
        
        return total;
    }
};
```

## 2315. Count Asterisks
```cpp
class Solution {
public:
    int countAsterisks(string s) {
        int count{0};

        bool inGroup{false};
        for (const auto& c : s)
        {
            if (c == '|')
            {
                inGroup = !inGroup;
                continue;
            }

            count += (c == '*' && !inGroup);
        }
        
        return count;
    }
};
```

## 1290. Convert Binary Number in a Linked List to Integer
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
    int getDecimalValue(ListNode* head) {
        int num{0};

        while (head)
        {
            num <<= 1;
            num |= head->val;
            head = head->next;
        }

        return num;
    }
};
```

## 1816. Truncate Sentence
```cpp
class Solution {
public:
    string truncateSentence(string s, int k) {
        int wordCount{0};
        for (int i{0}; i < s.size(); i++)
        {
            wordCount += s[i] == ' ';
            if (wordCount == k) return s.substr(0, i);
        }

        return s;
    }
};
```

## 2220. Minimum Bit Flips to Convert Number
```cpp
class Solution {
public:
    int minBitFlips(int start, int goal) {
        return __builtin_popcount(start ^ goal);
    }
};
```

## 2037. Minimum Number of Moves to Seat Everyone
```cpp
class Solution {
    vector<int> countingSort(const vector<int>& arr)
    {
        array<int, 101> buckets{0};
        for (const auto& a : arr)
        {
            buckets[a]++;
        }

        vector<int> result;
        for (int num{0}; num < buckets.size(); num++)
        {
            while (buckets[num])
            {
                result.push_back(num);
                buckets[num]--;
            }
        }

        return result;
    }

public:
    int minMovesToSeat(vector<int>& seats, vector<int>& students) {
        auto sortedSeats { countingSort(seats) };
        auto sortedStudents { countingSort(students) };

        int moves{0};
        for (int i{0}; i < sortedSeats.size(); i++)
        {
            moves += abs(sortedSeats[i] - sortedStudents[i]);
        }
        
        return moves;
    }
};
```

## 709. To Lower Case
```cpp
class Solution {
public:
    string toLowerCase(string s) {
        transform(s.begin(), s.end(), s.begin(), ::tolower);
        return s;
    }
};
```

## 1684. Count the Number of Consistent Strings
```cpp
class Solution {
public:
    int countConsistentStrings(string allowed, vector<string>& words) {
        int mask{0};
        for (auto& c : allowed)
        {
            mask |= 1 << (c - 'a');
        }

        int count{0};
        for (const auto& w : words)
        {
            count += all_of(begin(w), end(w), [&](const auto& c) {
                return (mask & (1 << (c - 'a')));
            });
        }

        return count;
    }
};
```

## 557. Reverse Words in a String III
```cpp
class Solution {
public:
    string reverseWords(string s) {
        int start{0};
        for (int end{0}; end <= s.size(); end++)
        {
            if (s[end] == ' ' || end == s.size())
            {
                reverse(s.begin() + start, s.begin() + end);
                start = end + 1;
            }
        }

        return s;
    }
};
```

## 1890. The Latest Login in 2020
```cpp
# Write your MySQL query statement below
SELECT user_id, 
       MAX(time_stamp) AS last_stamp
FROM Logins
WHERE YEAR(time_stamp) = 2020
GROUP BY user_id
```

## 2103. Rings and Rods
```cpp
class Solution {
private:
    const unordered_map<char, int> colorToBitOffset { {'G', 0}, {'B', 1}, {'R', 2} };

public:
    int countPoints(string rings) {
        int mask{0};
        for (int i{0}; i < rings.size(); i += 2)
        {
            const auto color{rings[i]};
            const auto rod{rings[i + 1] - '0'};
            mask |= (1 << (colorToBitOffset.at(color) + rod * 3));
        }

        int count{0};
        while (mask)
        {
            if ((0b111 & mask) == 0b111)
            {
                count++;
            }

            mask >>= 3;
        }

        return count;
    }
};
```

## 2391. Minimum Amount of Time to Collect Garbage
```cpp
class Solution {
public:
    int garbageCollection(vector<string>& garbage, vector<int>& travel) {
        auto minutes = 0;
        auto mask = 0;

        for (int house = garbage.size() - 1; house >= 0; --house) {
            minutes += garbage[house].size();

            for (const auto type : garbage[house]) {
                const auto bitPosition = type - 'A';
                mask |= 1 << bitPosition;
            }

            if (house == 0) continue;

            const auto trucks = __builtin_popcount(mask);
            minutes += travel[house - 1] * trucks;
        }

        return minutes;
    }
};
```

## 2130. Maximum Twin Sum of a Linked List
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
private:
    ListNode* getMiddleNode(ListNode* node)
    {
        auto fast {node->next};
        auto slow {node};

        while (fast && fast->next)
        {
            fast = fast->next->next;
            slow = slow->next;
        }

        return slow;
    }

    ListNode* reverse(ListNode* head)
    {
        ListNode* prev = nullptr;
        ListNode* curr = head;
        ListNode* next = nullptr;

        while (curr)
        {
            next = curr->next;
            curr->next = prev;
            prev = curr;
            curr = next;
        }

        return prev;
    }

public:
    int pairSum(ListNode* head) {
        stack<int> s;
        
        // Split list in half
        auto firstHalfTail {getMiddleNode(head)};
        auto secondHalfHead = firstHalfTail->next;
        firstHalfTail->next = nullptr;

        // Reverse second half
        auto reversedSecondHalfHead {reverse(secondHalfHead)};

        // Process both halves in lock step
        int maxTwinSum = 0;
        auto node1 {head};
        auto node2 {reversedSecondHalfHead};
        while (node1 && node2)
        {
            maxTwinSum = max(maxTwinSum, node1->val + node2->val);
            node1 = node1->next;
            node2 = node2->next;
        }

        // Put list back together
        firstHalfTail->next = reverse(reversedSecondHalfHead);

        return maxTwinSum;
    }
};
```

## 1859. Sorting the Sentence
```cpp
class Solution {
public:
    string sortSentence(string s) {
        string result;
        array<string, 10> buckets;

        string currWord;
        int currIndex = 0;
        for (auto i = 0; i <= s.size(); i++)
        {
            if (s[i] == ' ' || i == s.size())
            {
                buckets[currIndex] = currWord;
                currWord = "";
                currIndex = 0;
                continue;
            }

            if (isalpha(s[i]))
            {
                currWord += s[i];
            }
            else if (isdigit(s[i]))
            {
                currIndex *= 10;
                currIndex += s[i] - '0';
            }
        }

        for (const auto& s : buckets)
        {
            if (s != "") 
            {
                if (result.size())
                {
                    result += " ";
                }

                result += s;
            }
        }

        return result;
    }
};
```

## 1773. Count Items Matching a Rule
```cpp
class Solution {
    const unordered_map<string, int> ruleIndexMap {{"type", 0}, {"color", 1}, {"name", 2}};
public:
    int countMatches(vector<vector<string>>& items, string ruleKey, string ruleValue) {
        int count = 0;
        const int ruleIndex = ruleIndexMap.at(ruleKey);

        for (const auto& item : items)
        {
            if (item[ruleIndex] == ruleValue)
            {
                count++;
            }
        }

        return count;
    }
};
```

## 1817. Finding the Users Active Minutes
```cpp
class Solution {
public:
    vector<int> findingUsersActiveMinutes(vector<vector<int>>& logs, int k) {
        vector<int> result(k);

        unordered_map<int, unordered_set<int>> userToUniqueMinutes;
        for (const auto& log : logs)
        {
            int id = log[0];
            int min = log[1];
            userToUniqueMinutes[id].insert(min);
        }

        for (const auto& [user, uniqueMinutes] : userToUniqueMinutes)
        {
            result[uniqueMinutes.size() - 1]++;
        }

        return result;
    }
};
```

## 1021. Remove Outermost Parentheses
```cpp
class Solution {
public:
    string removeOuterParentheses(string s) {
        string result;

        int balance = 0;
        for (const auto& c : s)
        {
            if (c == '(')
            {
                // don't add first parenthesis (balance == 0)
                if (balance > 0)
                {
                    result += c;
                }

                balance++;
            }
            else if (c == ')')
            {
                balance--;

                // don't add last parenthesis (balance == 0)
                if (balance > 0)
                {
                    result += c;
                }
            }
        }

        return result;
    }
};
```

## 1877. Minimize Maximum Pair Sum in Array
```cpp
class Solution {
public:
    int minPairSum(vector<int>& nums) {
        sort(begin(nums), end(nums));

        int minMax = 0;
        int left = 0;
        int right = nums.size() - 1;
        while (left < right)
        {
            minMax = max(minMax, nums[left] + nums[right]);
            left++;
            right--;
        }

        return minMax;
    }
};
```

## 2079. Watering Plants
```cpp
class Solution {
public:
    int wateringPlants(vector<int>& plants, int capacity) {
        int currCapacity = capacity;
        int steps = 0;
        for (int i = 0; i < plants.size(); i++)
        {
            // step to next plant
            steps++;
            
            // if can not at necessary amount
            if (currCapacity < plants[i])
            {
                // steps to river + steps to get back - from one before curr plant
                steps += i * 2;
                // capacity now full
                currCapacity = capacity;
            }

            // water plant - decrease capacity
            currCapacity -= plants[i];
        }

        return steps;
    }
};
```

## 1572. Matrix Diagonal Sum
```cpp
class Solution {
public:
    int diagonalSum(vector<vector<int>>& mat) {
        int sum = 0;
        for (int i = 0; i < mat.size(); i++)
        {
            int primaryRow = i;
            int primaryCol = i;

            int secondaryRow = i;
            int secondaryCol = mat.size() - 1 - i;

            sum += mat[primaryRow][primaryCol];

            // Only add center to sum once
            if (primaryCol == secondaryCol)
            {
                continue;
            }
            
            sum += mat[secondaryRow][secondaryCol];
        }

        return sum;
    }
};
```

## 763. Partition Labels
```cpp
class Solution {
public:
    vector<int> partitionLabels(string s) {
        unordered_map<char, int> m;
        for (int i = 0; i < s.size(); i++) {
            m[s[i]] = i;
        }

        vector<int> result;
        int start = 0;
        int end = 0;
        for (int i = 0; i < s.size(); i++) {
            end = max(end, m[s[i]]);
            if (end == i) {
                result.push_back(end - start + 1);
                start = end + 1;
            }
        }

        return result;
    }
};
```

## 1844. Replace All Digits with Characters
```cpp
class Solution {
public:
    string replaceDigits(string s) {
        string result;

        for (int i = 0; i < s.size(); i += 2) {
            char c = s[i];
            result += c;

            if (i + 1 >= s.size()) {
                continue;
            }

            int digit = s[i + 1] - '0';        
            char shiftedC = c + digit;
            result += shiftedC;
        }

        return result;
    }
};
```

## 1557. Minimum Number of Vertices to Reach All Nodes
```cpp
class Solution {
public:
    vector<int> findSmallestSetOfVertices(int n, vector<vector<int>>& edges) {
        vector<int> indegree(n);
        for (const auto& edge : edges) {
            indegree[edge[1]]++;
        }

        vector<int> result;
        for (int node = 0; node < n; node++) {
            if (indegree[node] == 0) {
                result.push_back(node);
            }
        }
        return result;
    }
};
```

## 1464. Maximum Product of Two Elements in an Array
```cpp
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int max1 = 0;
        int max2 = 0;

        for (const auto& n : nums) {
            if (max1 < n) {
                max2 = max1;
                max1 = n;
            } else if (max2 < n) {
                max2 = n;
            }
        }

        return (max1 - 1) * (max2 - 1);
    }
};
```

## 2485. Find the Pivot Integer
```cpp
class Solution {
public:
    int pivotInteger(int n) {
        // gauss summation
        const int totalSum = n * (n + 1) / 2;
        const int square = std::sqrt(totalSum);
        return square * square == totalSum ? square : -1;
    }
};
```

## 2331. Evaluate Boolean Binary Tree
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
    bool evaluateTree(TreeNode* root) {
        if (!root->left && !root->right) {
            return root->val;
        }

        bool left = evaluateTree(root->left);
        bool right = evaluateTree(root->right);

        // OR
        if (root->val == 2) {
            return left || right;
        }
        // AND
        else {
            return left && right;
        }
    }
};
```

## 1266. Minimum Time Visiting All Points
```cpp
class Solution {
public:
    int minTimeToVisitAllPoints(vector<vector<int>>& points) {
        int time = 0;
        
        for (int i = 0; i < points.size() - 1; i++) {
            auto point1 = points[i];
            int point1X = point1[0];
            int point1Y = point1[1];

            auto point2 = points[i + 1];
            int point2X = point2[0];
            int point2Y = point2[1];

            // Chebyshev distance
            int xDelta = abs(point1X - point2X);
            int yDelta = abs(point1Y - point2Y);
            time += max(xDelta, yDelta);
        }
        
        return time;
    }
};
```

## 2482. Difference Between Ones and Zeros in Row and Column
```cpp
class Solution {
public:
    vector<vector<int>> onesMinusZeros(vector<vector<int>>& grid) {
        vector<int> onesRow(grid.size());
        vector<int> onesCol(grid[0].size());
        vector<int> zerosRow(grid.size());
        vector<int> zerosCol(grid[0].size());

        for (int row = 0; row < grid.size(); row++) {
            for (int col = 0; col < grid[0].size(); col++) {
                if (grid[row][col] == 1) {
                    onesRow[row]++;
                    onesCol[col]++;
                } else {
                    zerosRow[row]++;
                    zerosCol[col]++;
                }
            }
        }

        for (int row = 0; row < grid.size(); row++) {
            for (int col = 0; col < grid[0].size(); col++) {
                grid[row][col] = onesRow[row] + onesCol[col] - zerosRow[row] - zerosCol[col];
            }
        }

        return grid;
    }
};
```

## 2221. Find Triangular Sum of an Array
```cpp
class Solution {
public:
    int triangularSum(vector<int>& nums) {
        int n = nums.size();
        while (n > 1) {
            for (int i = 0; i < n - 1; i++) {
                nums[i] = (nums[i] + nums[i + 1]) % 10;
            }

            n--;
        }

        return nums[0];
    }
};
```

## 2442. Count Number of Distinct Integers After Reverse Operations
```cpp
class Solution {
private:
    int reverse(int n) {
        int result = 0;
        
        while (n) {
            result *= 10;
            result += n % 10;
            n /= 10;
        }
        
        return result;
    }
public:
    int countDistinctIntegers(vector<int>& nums) {
        unordered_set<int> seen;

        for (const auto& n : nums) {
            seen.insert(n);
            seen.insert(reverse(n));
        }

        return seen.size();
    }
};
```

## 1725. Number Of Rectangles That Can Form The Largest Square
```cpp
class Solution {
public:
    int countGoodRectangles(vector<vector<int>>& rectangles) {
        int maxLen = 0;
        int maxCount = 0;

        for (const auto& r : rectangles) {
            int currMaxLen = min(r[0], r[1]);
            
            if (maxLen < currMaxLen) {
                maxLen = currMaxLen;
                maxCount = 1;
            } else if (maxLen == currMaxLen) {
                maxCount++;
            }
        }

        return maxCount;
    }
};
```

## 1732. Find the Highest Altitude
```cpp
class Solution {
public:
    int largestAltitude(vector<int>& gain) {
        int highestAltitude = 0;
        int altitude = 0;
        
        for (const auto g : gain) {
            altitude += g;
            highestAltitude = std::max(highestAltitude, altitude);
        }

        return highestAltitude;
    }
};
```

## 1561. Maximum Number of Coins You Can Get
```cpp
class Solution {
public:
    int maxCoins(vector<int>& piles) {
        sort(piles.begin(), piles.end());

        int sum = 0;
        for (int i = piles.size() / 3; i < piles.size(); i += 2) {
            sum += piles[i];
        }
        
        return sum;
    }
};
```

## 2108. Find First Palindromic String in the Array
```cpp
class Solution {
public:
    string firstPalindrome(vector<string>& words) {
        for (const auto& word : words) {
            if (isPalindrome(word)) {
                return word;
            }
        }

        return "";
    }

private:
    bool isPalindrome(const string& s) {
        int left = 0;
        int right = s.size() - 1;

        while (left < right) {
            if (s[left] != s[right]) {
                return false;
            }

            left++;
            right--;
        }
        
        return true;
    }
};
```

## 1827. Minimum Operations to Make the Array Increasing
```cpp
class Solution {
public:
    int minOperations(vector<int>& nums) {
        int currMax = 0;
        int operations = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (nums[i] > currMax) {
                currMax = nums[i];
                continue;
            }

            int newVal = nums[i] + (currMax - nums[i] + 1);
            operations += currMax - nums[i] + 1;
            currMax = newVal;            
        }

        return operations;
    }
};
```

## 890. Find and Replace Pattern
```cpp
class Solution {
private:
    bool isMatch(string word, string pattern) {
        unordered_map<char, char> wToP;
        unordered_map<char, char> pToW;

        for (int i = 0; i < word.size(); i++) {
            if (wToP.count(word[i]) == 0 && pToW.count(pattern[i]) == 0) {
                wToP[word[i]] = pattern[i];
                pToW[pattern[i]] = word[i];
                continue;
            }

            if (wToP[word[i]] != pattern[i] || pToW[pattern[i]] != word[i]) {
                return false;
            }
        }

        return true;
    }

public:
    vector<string> findAndReplacePattern(vector<string>& words, string pattern) {
        vector<string> result;

        for (auto& word : words) {
            if (isMatch(word, pattern)) {
                result.push_back(word);
            }
        }

        return result;
    }
};
```

## 2000. Reverse Prefix of Word
```cpp
class Solution {
public:
    string reversePrefix(string word, char ch) {
        auto index = word.find(ch);
        // string::npos == -1, -1 + 1 = 0, reverse(0, 0) - if char not in word
        reverse(word.begin(), word.begin() + index + 1);
        return word;
    }
};
```

## 728. Self Dividing Numbers
```cpp
class Solution {
private:
    bool isSelfDividing(int num) {
        int n = num;
        while (n) {
            int digit = n % 10;
            if (digit == 0) {
                return false;
            }

            if (num % digit != 0) {
                return false;
            }

            n /= 10;
        }
        return true;
    }

public:
    vector<int> selfDividingNumbers(int left, int right) {
        vector<int> result;
        for (int num = left; num <= right; num++) {
            if (isSelfDividing(num)) {
                result.push_back(num);
            }
        }
        return result;
    }
};
```

## 1436. Destination City
```cpp
class Solution {
public:
    string destCity(vector<vector<string>>& paths) {
        unordered_map<string, int> outdegree;

        for (const auto& path : paths) {
            if (outdegree.count(path[0]) == 0) {
                outdegree[path[0]] = 0;
            }

            if (outdegree.count(path[1]) == 0) {
                outdegree[path[1]] = 0;
            }

            outdegree[path[0]]++;
        }

        for (const auto& [key, value] : outdegree) {
            if (value == 0) {
                return key;
            }
        }

        return "";
    }
};
```

## 1374. Generate a String With Characters That Have Odd Counts
```cpp
class Solution {
public:
    string generateTheString(int n) {
        if (n % 2 == 0) {
            return std::string(n - 1, 'a') + 'b';
        } else {
            return std::string(n, 'a');
        }
    }
};
```

## 1347. Minimum Number of Steps to Make Two Strings Anagram
```cpp
class Solution {
public:
    int minSteps(string s, string t) {
        vector<int>count(26);
        
        for (const auto& c : s) {
            count[c - 'a']++;
        }

        for (const auto& c : t) {
            count[c - 'a']--;
        }

        int diff = 0;
        for (int i = 0; i < 26; i++) {
            diff += max(count[i], 0);
        }
        return diff;
    }
};
```

## 1812. Determine Color of a Chessboard Square
```cpp
class Solution {
public:
    bool squareIsWhite(string coordinates) {
        int row = coordinates[1] - '0';
        int col = coordinates[0] - 'a';

        // is odd, starts as black
        if (row & 1) {
            return col & 1;
        }
        // is even, starts as white
        else {
            return !(col & 1);
        }
    }
};
```

## 1370. Increasing Decreasing String
```cpp
class Solution {
public:
    string sortString(string s) {
        vector<int> counts(26);
        for (const auto& c : s) {
            counts[c - 'a']++;
        }

        string result;
        while (result.size() != s.size()) {
            // forward
            for (int i = 0; i < counts.size(); i++) {
                if (counts[i]) {
                    result += i + 'a';
                    counts[i]--;
                }
            }

            if (result.size() == s.size()) break;

            // reverse
            for (int i = counts.size() - 1; i >= 0; i--) {
                if (counts[i]) {
                    result += i + 'a';
                    counts[i]--;       
                }
            }
        }

        return result;
    }
};
```

## 700. Search in a Binary Search Tree
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
    TreeNode* searchBST(TreeNode* root, int val) {
        while (root) {
            if (root->val == val) {
                return root;
            } else if (root->val < val) {
                root = root->right;
            } else {
                root = root->left;
            }
        }
        return nullptr;
    }
};
```

## 1304. Find N Unique Integers Sum up to Zero
```cpp
class Solution {
public:
    vector<int> sumZero(int n) {
        vector<int> result;
        for (int i = 1; i <= n / 2; i++) {
            result.push_back(-i);
            result.push_back(i);
        }

        if (result.size() != n) {
            result.push_back(0);
        }

        return result;
    }
};
```

## 2185. Counting Words With a Given Prefix
```cpp
class Solution {
public:
    int prefixCount(vector<string>& words, string pref) {
        int count = 0;
        for (const auto& word : words) {
            if (word.find(pref) == 0) {
                count++;
            }
        }
        return count;
    }
};
```

## 1295. Find Numbers with Even Number of Digits
```cpp
class Solution {
public:
    int findNumbers(vector<int>& nums) {
        int evenNumCount = 0;
        for (const auto& n : nums) {
            int digitCount = log10(n) + 1;
            evenNumCount += digitCount % 2 == 0;
        }
        return evenNumCount;
    }
};
```

## 1941. Check if All Characters Have Equal Number of Occurrences
```cpp
class Solution {
public:
    bool areOccurrencesEqual(string s) {
        vector<int> counts(26);
        for (int i = 0; i < s.size(); i++) {
            counts[s[i] - 'a']++;
        }

        int freq = 0;
        for (int i = 0; i < counts.size(); i++) {
            if (counts[i] > 0) {
                if (freq == 0) {
                    freq = counts[i];
                }
                
                if (counts[i] != freq) {
                    return false;
                }
            }
        }

        return true;
    }
};
```

## 2341. Maximum Number of Pairs in Array
```cpp
class Solution {
public:
    vector<int> numberOfPairs(vector<int>& nums) {
        vector<int> counts(101);

        for (int i = 0; i < nums.size(); i++) {
            counts[nums[i]]++;
        }

        int pairs = 0;
        int remaining = 0;

        for (int i = 0; i < 101; i++) {
            pairs += counts[i] / 2;
            remaining += counts[i] % 2;
        }

        return {pairs, remaining};
    }
};
```

## 2418. Sort the People
```cpp
class Solution {
public:
    vector<string> sortPeople(vector<string>& names, vector<int>& heights) {
        vector<pair<int, string>> people;

        for (int i = 0; i < names.size(); i++) {
            people.push_back({heights[i], names[i]});
        }

        sort(people.begin(), people.end(), [](const auto& a, const auto& b) {
            return a > b;
        });

        vector<string> result;
        for (const auto& [height, name] : people) {
            result.push_back(name);
        }
        return result;
    }
};
```

## 2089. Find Target Indices After Sorting Array
### Dutch Flag
```cpp
class Solution {
public:
    vector<int> targetIndices(vector<int>& nums, int target) {
        int lessThanTarget = 0;
        int notYetSorted = 0;
        int greaterThanTarget = nums.size() - 1;

        while (notYetSorted <= greaterThanTarget) {
            if (nums[notYetSorted] > target) {
                swap(nums[notYetSorted], nums[greaterThanTarget]);
                greaterThanTarget--;
            } else if (nums[notYetSorted] < target) {
                swap(nums[lessThanTarget], nums[notYetSorted]);
                lessThanTarget++;
                notYetSorted++;
            } else {
                notYetSorted++;
            }
        }

        vector<int> result;
        for (int i = lessThanTarget; i < notYetSorted; i++) {
            result.push_back(i);
        }
        return result;
    }
};
```

### Count
```cpp
class Solution {
public:
    vector<int> targetIndices(vector<int>& nums, int target) {
        int lessThanTargetCount = 0;
        int equalToTargetCount = 0;

        for (const auto& num : nums) {
            lessThanTargetCount += num < target;
            equalToTargetCount += num == target;
        }
        
        vector<int> result;
        for (int i = lessThanTargetCount; 
             i < lessThanTargetCount + equalToTargetCount; 
             i++) {
            result.push_back(i);
        }
        return result;
    }
};
```

## 938. Range Sum of BST
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
    int rangeSumBST(TreeNode* root, int low, int high) {
        if (!root) return 0;
        
        if (root->val < low) {
            return rangeSumBST(root->right, low, high);
        }

        if (root->val > high) {
            return rangeSumBST(root->left, low, high);
        }

        return root->val + 
            rangeSumBST(root->left, low, root->val) + 
            rangeSumBST(root->right, root->val, high);
    }
};
```

## 1913. Maximum Product Difference Between Two Pairs
```cpp
class Solution {
public:
    int maxProductDifference(vector<int>& nums) {
        int max1 = INT_MIN;
        int max2 = INT_MIN;

        int min1 = INT_MAX;
        int min2 = INT_MAX;

        for (const auto& n : nums) {
            if (n > max1) {
                max2 = max1;
                max1 = n;
            } else if (n > max2) {
                max2 = n;
            }

            if (n < min1) {
                min2 = min1;
                min1 = n;
            } else if (n < min2) {
                min2 = n;
            }
        }

        return max1 * max2 - min1 * min2;
    }
};
```

## 344. Reverse String
```cpp
class Solution {
public:
    void reverseString(vector<char>& s) {
        int i = 0;
        int j = s.size() - 1;

        while (i < j) {
            swap(s[i], s[j]);
            i++;
            j--;
        }
    }
};
```

## 921. Minimum Add to Make Parentheses Valid
```cpp
class Solution {
public:
    int minAddToMakeValid(string s) {
        int moves = 0;
        int balance = 0;
        
        for (const auto& c : s) {
            if (c == '(') {
                if (balance < 0) {
                    moves += abs(balance);
                    balance = 0;
                }
                balance++;
            } else if (c == ')') {
                balance--;
            }
        }

        moves += abs(balance);
        return moves;
    }
};
```

## 1768. Merge Strings Alternately
```cpp
class Solution {
public:
    string mergeAlternately(string word1, string word2) {
        string result;

        int i = 0;
        int j = 0;
        while (i < word1.size() && j < word2.size()) {
            result += word1[i];
            result += word2[j];
            i++;
            j++;
        }

        if (i < word1.size()) {
            result += word1.substr(i);
        } else {
            result += word2.substr(j);
        }

        return result;
    }
};
```

## 198. House Robber
```cpp
class Solution {
public:
    int rob(vector<int>& nums) {
        if (nums.size() == 1) {
            return nums[0];
        }

        vector<int> dp(nums.size());
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        
        for (int i = 2; i < nums.size(); i++) {
            dp[i] = max(dp[i - 1], nums[i] + dp[i - 2]);
        }

        return dp[nums.size() - 1];
    }
};
```

## 2351. First Letter to Appear Twice
```cpp
class Solution {
public:
    char repeatedCharacter(string s) {
        int mask = 0;
        for (const auto& c : s) {
            int bin = 1 << (c - 'a');
            if ((mask & bin) != 0) {
                return c;
            }

            mask |= bin;
        }

        return 0;
    }
};
```

## 1450. Number of Students Doing Homework at a Given Time
```cpp
class Solution {
public:
    int busyStudent(vector<int>& startTime, vector<int>& endTime, int queryTime) {
        int count = 0;
        for (int i = 0; i < startTime.size(); i++) {
            if (startTime[i] <= queryTime && queryTime <= endTime[i]) {
                count++;
            }
        }

        return count;
    }
};
```

## 2119. A Number After a Double Reversal
```cpp
class Solution {
public:
    bool isSameAfterReversals(int num) {
        if (num == 0) return true;
        // if trailing zero then false
        return (num % 10) != 0;
    }
};
```

## 1748. Sum of Unique Elements
```cpp
class Solution {
public:
    int sumOfUnique(vector<int>& nums) {
        vector<int> counts(101);

        for (const auto& n : nums) {
            counts[n]++;
        }

        int sum = 0;
        for (int num = 0; num < counts.size(); num++) {
            if (counts[num] == 1) {
                sum += num;
            }
        }
        return sum;
    }
};
```

## 905. Sort Array By Parity
```cpp
class Solution {
public:
    vector<int> sortArrayByParity(vector<int>& nums) {
        int i = 0;
        for (int j = 0; j < nums.size(); j++) {
            if (nums[j] % 2 == 0) {
                swap(nums[i], nums[j]);
                i++;
            }
        }      

        return nums;
    }
};
```

## 1475. Final Prices With a Special Discount in a Shop
```cpp
class Solution {
public:
    vector<int> finalPrices(vector<int>& prices) {
        vector<int> result = prices;
        vector<int> stack;
        for (int i = 0; i < prices.size(); i++) {
            while (stack.size() && prices[stack.back()] >= prices[i]) {
                int index = stack.back();
                stack.pop_back();
                result[index] = prices[index] - prices[i];
            }

            stack.push_back(i);
        }
        
        return result;
    }
};
```

## 237. Delete Node in a Linked List
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    void deleteNode(ListNode* node) {
        auto nextNode = node->next;
        node->val = nextNode->val;
        node->next = nextNode->next;
        delete nextNode;
    }
};
```

## 657. Robot Return to Origin
```cpp
class Solution {
public:
    bool judgeCircle(string moves) {
        // if odd number of moves, impossible to return to origin
        if (moves.size() & 1) {
            return false;
        }

        int x = 0;
        int y = 0;

        for (const auto c : moves) {
            if (c == 'U') {
                y++;
            } else if (c == 'D') {
                y--;
            } else if (c == 'L') {
                x--;
            } else if (c == 'R') {
                x++;
            }
        }

        return x == 0 && y == 0;
    }
};
```

## 2363. Merge Similar Items
```cpp
class Solution {
public:
    vector<vector<int>> mergeSimilarItems(vector<vector<int>>& items1, vector<vector<int>>& items2) {
        vector<int> buckets(1001);
        for (const auto& item : items1) {
            auto weight = item[0];
            auto value = item[1];
            buckets[weight] += value;
        }

        for (const auto& item : items2) {
            auto weight = item[0];
            auto value = item[1];
            buckets[weight] += value;
        }

        vector<vector<int>> result;
        for (int value = 0; value < buckets.size(); value++) {
            auto sum = buckets[value];
            if (sum == 0) continue;
            result.push_back({value, buckets[value]});
        }
        return result;
    }
};
```

## 1351. Count Negative Numbers in a Sorted Matrix
```cpp
class Solution {
public:
    int countNegatives(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();
        
        int total = 0;
        
        int currRow = 0;
        int currCol = n - 1;
        while (currRow < m && currCol >= 0) {
            if (grid[currRow][currCol] < 0) {
                total += m - currRow;
                currCol--;
                continue;
            }

            currRow++;
        }
        return total;
    }
};
```

## 1051. Height Checker
```cpp
class Solution {
public:
    int heightChecker(vector<int>& heights) {
        vector<int> buckets(101);
        for (const auto height : heights) {
            buckets[height]++;
        }

        vector<int> sorted;
        for (int num = 0; num < buckets.size(); num++) {
            while (buckets[num]--) {
                sorted.push_back(num);
            }
        }

        int count = 0;
        for (int i = 0; i < heights.size(); i++) {
            if (sorted[i] != heights[i]) {
                count++;
            }
        }

        return count;
    }
};
```

## 150. Evaluate Reverse Polish Notation
```cpp
class Solution {
private:
    unordered_map<string, function<long (long, long)>> operators {
        {"+", [](const long a, const long b) { return a + b; }},
        {"-", [](const long a, const long b) { return a - b; }},
        {"*", [](const long a, const long b) { return a * b; }},
        {"/", [](const long a, const long b) { return a / b; }},
    };

public:
    int evalRPN(vector<string>& tokens) {
        vector<long> stack;
        for (const auto& token : tokens) {
            // Not an operator
            if (operators.count(token) == 0) {
                stack.push_back(stoi(token));
                continue;
            }

            const long num2 = stack.back();
            stack.pop_back();
            const long num1 = stack.back();
            stack.pop_back();

            stack.push_back(operators[token](num1, num2));
        }

        return stack.back();
    }
};
```

## 46. Permutations
```cpp
class Solution {
private: 
    void _permute(vector<vector<int>>& result, vector<int>& nums, int index) {
        if (index >= nums.size()) {
            result.push_back(nums);
            return;
        }

        for (int i = index; i < nums.size(); i++) {
            // swap
            swap(nums[index], nums[i]);
            _permute(result, nums, index + 1);
            // unswap
            swap(nums[index], nums[i]);
        }
    }
public:
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> result;
        _permute(result, nums, 0);
        return result;   
    }
};
```

## 861. Score After Flipping Matrix
```cpp
class Solution {
private:
    void flipRow(vector<vector<int>>& grid, int row) {
        for (int col = 0; col < grid[0].size(); col++) {
            grid[row][col] ^= 1;
        }
    }

    void flipCol(vector<vector<int>>& grid, int col) {
        for (int row = 0; row < grid.size(); row++) {
            grid[row][col] ^= 1;
        }
    }

    int getNum(vector<vector<int>>& grid, int row) {
        int bin = 0;
        int shift = 0;
        for (int col = grid[0].size() - 1; col >= 0; col--) {
            bin |= (grid[row][col] << shift);
            shift++;
        }
        return bin;
    }

public:
    int matrixScore(vector<vector<int>>& grid) {
        for (int row = 0; row < grid.size(); row++) {
            if (grid[row][0] == 0) {
                flipRow(grid, row);
            }
        }

        for (int col = 1; col < grid[0].size(); col++) {
            int zeroCount = 0;
            int oneCount = 0;
            for (int row = 0; row < grid.size(); row++) {
                if (grid[row][col] == 1) {
                    oneCount++;
                } else {
                    zeroCount++;
                }
            }

            if (zeroCount > oneCount) {
                flipCol(grid, col);
            }
        }

        int sum = 0;
        for (int row = 0; row < grid.size(); row++) {
            sum += getNum(grid, row);
        }
        return sum;
    }
};
```

## 461. Hamming Distance
### Built In
```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        return __builtin_popcount(x ^ y);
    }
};
```

### Bit Manipulation
```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        int hamming = x ^ y;
        int count = 0;
        while (hamming) {
            hamming &= (hamming - 1);
            count++;
        }
        return count;
    }
};
```

## 419. Battleships in a Board
```cpp
class UnionFind {
public:
    UnionFind(int n): m_numOfComponents(n) {
        for (int i = 0; i < n; i++) {
            m_parents.push_back(i);
            m_sizes.push_back(1);
        }
    }

    void unionElements(int a, int b) {
        int parentA = find(a);
        int parentB = find(b);

        if (parentA == parentB) {
            return;
        }

        if (m_sizes[parentA] < m_sizes[parentB]) {
            m_sizes[parentB] += m_sizes[parentA];
            m_parents[parentA] = parentB;
        } else {
            m_sizes[parentA] += m_sizes[parentB];
            m_parents[parentB] = parentA;
        }

        m_numOfComponents--;
    }

    int find(int a) {
        int root = a;
        while (root != m_parents[root]) {
            root = m_parents[root];
        }

        while (a != root) {
            auto next = m_parents[a];
            m_parents[a] = root;
            a = next;
        }

        return root;
    }

    int size() {
        return m_numOfComponents;
    }
private:
    int m_numOfComponents;
    vector<int> m_parents;
    vector<int> m_sizes;
};

class Solution {
public:
    int countBattleships(vector<vector<char>>& board) {
        const int m = board.size();
        const int n = board[0].size();
        const vector<pair<int, int>> dirs {{1, 0}, {0, 1}, {-1, 0}, {0, -1}};
        UnionFind unionFind(m * n);
        int emptySpaces = 0;

        for (int row = 0; row < m; row++) {
            for (int col = 0; col < n; col++) {
                if (board[row][col] == '.') {
                    emptySpaces++;
                    continue;
                }

                for (const auto& [deltaRow, deltaCol] : dirs) {
                    const auto nextRow = deltaRow + row;
                    const auto nextCol = deltaCol + col;

                    if (nextRow < 0 || nextRow >= m || 
                        nextCol < 0 || nextCol >= n) {
                        continue;
                    }

                    if (board[nextRow][nextCol] == 'X') {
                        unionFind.unionElements(row * n + col, nextRow * n + nextCol);
                    }
                }
            }
        }
        return unionFind.size() - emptySpaces;
    }
};
```

## 1325. Delete Leaves With a Given Value
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
private:
    TreeNode* _removeLeafNodes(TreeNode* root, int target) {
        if (!root) return nullptr;

        root->left = _removeLeafNodes(root->left, target);
        root->right = _removeLeafNodes(root->right, target);

        if (!root->left && !root->right && root->val == target) {
            // delete root; // should delete free node. but online judge crashes
            return nullptr;
        }

        return root;
    }

public:
    TreeNode* removeLeafNodes(TreeNode* root, int target) {
        return _removeLeafNodes(root, target);
    }
};
```

## 2206. Divide Array Into Equal Pairs
```cpp
class Solution {
public:
    bool divideArray(vector<int>& nums) {
        unordered_map<int, int> m;
        for (const int num : nums) {
            m[num]++;
        }

        for (const auto& [key, val] : m) {
            if (val % 2 != 0) {
                return false;
            }
        }

        return true;
    }
};
```

## 1299. Replace Elements with Greatest Element on Right Side
```cpp
class Solution {
public:
    vector<int> replaceElements(vector<int>& arr) {
        vector<int> result(arr);
        int maxNum = -1;
        for (int i = arr.size() - 1; i >= 0; i--) {
            result[i] = maxNum;
            maxNum = max(maxNum, arr[i]);
        }
        return result;
    }
};
```

## 739. Daily Temperatures
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        vector<int> result(temperatures.size(), 0);
        vector<int> stack;

        for (int i = 0; i < temperatures.size(); i++) {
            while (stack.size() && temperatures[stack.back()] < temperatures[i]) {
                int index = stack.back();
                stack.pop_back();

                result[index] = i - index;
            }

            stack.push_back(i);
        }

        return result;      
    }
};
```

## 701. Insert into a Binary Search Tree
### Recursive
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
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (!root) {
            return new TreeNode(val);
        }

        if (root->val < val) {
            root->right = insertIntoBST(root->right, val);
        } else {
            root->left = insertIntoBST(root->left, val);
        }

        return root;
    }
};
```

### Iterative
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
    TreeNode* insertIntoBST(TreeNode* root, int val) {
        if (!root) {
            return new TreeNode(val);
        }

        TreeNode* prev = nullptr;
        TreeNode* curr = root;
        while (curr) {
            prev = curr;
            if (curr->val < val) {
                curr = curr->right;
            } else {
                curr = curr->left;
            }
        }

        TreeNode* node = new TreeNode(val);
        if (prev->val < val) {
            prev->right = node;
        } else {
            prev->left = node;
        }

        return root;
    }
};
```

## 2326. Spiral Matrix IV
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
    vector<vector<int>> spiralMatrix(int m, int n, ListNode* head) {
        vector<vector<int>> matrix(m, std::vector<int>(n, -1));

        int top = 0;
        int bottom = m - 1;
        int left = 0;
        int right = n - 1;

        ListNode* curr = head;

        while (curr && top <= bottom) {
            // up
            for (int col = left; curr && col <= right; col++) {
                matrix[top][col] = curr->val;
                curr = curr->next;
            }
            if (!curr) break;
            top++;
            
            // right
            for (int row = top; curr && row <= bottom; row++) {
                matrix[row][right] = curr->val;
                curr = curr->next;
            }
            if (!curr) break;
            right--;

            // bottom
            for (int col = right; curr && col >= left; col--) {
                matrix[bottom][col] = curr->val;
                curr = curr->next;
            }
            if (!curr) break;
            bottom--;

            // left
            for (int row = bottom; curr && row >= top; row--) {
                matrix[row][left] = curr->val;
                curr = curr->next;
            }
            if (!curr) break;
            left++;
        }

        return matrix;
    }
};
```

## 2405. Optimal Partition of String
```cpp
class Solution {
public:
    int partitionString(string s) {
        int partitions = 1;
        int seen = 0;
        for (int i = 0; i < s.size(); i++) {
            int bin = 1 << (s[i] - 'a');

            // if seen before
            if (seen & bin) {
                partitions++;
                seen = 0;
            }

            seen |= bin;
        }

        return partitions;
    }
};
```

## 1277. Count Square Submatrices with All Ones
```cpp
class Solution {
public:
    int countSquares(vector<vector<int>>& matrix) {
        for (int row = 1; row < matrix.size(); row++) {
            for (int col = 1; col < matrix[0].size(); col++) {
                if (!matrix[row][col]) {
                    continue;
                }

                matrix[row][col] = 1 + min(matrix[row-1][col-1], 
                                       min(matrix[row-1][col], 
                                       matrix[row][col-1]));
            }
        }

        int count = 0;
        for (int row = 0; row < matrix.size(); row++) {
            for (int col = 0; col < matrix[0].size(); col++) {
                count += matrix[row][col];
            }
        }
        return count;
    }
};
```

## 78. Subsets
### Recursive
```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> result;
        vector<int> curr;
        _subsets(nums, result, curr, 0);
        return result;
    }

private:
    void _subsets(vector<int>& nums, vector<vector<int>>& result, vector<int>& curr, int index) {
        if (index >= nums.size()) {
            result.push_back(curr);
            return;
        }

        // take
        curr.push_back(nums[index]);
        _subsets(nums, result, curr, index + 1);
        curr.pop_back();

        // don't take
        _subsets(nums, result, curr, index + 1);
    }
};
```

### Iterative Bitmask
```cpp
class Solution {
public:
    vector<vector<int>> subsets(vector<int>& nums) {
        vector<vector<int>> result;

        int upperBound = (1 << nums.size()) - 1;
        for (int bin = 0; bin <= upperBound; bin++) {
            vector<int> curr;
            for (int i = 0; i < nums.size(); i++) {
                int mask = 1 << i;
                if (bin & mask) {
                    curr.push_back(nums[i]);
                }
            }
            result.push_back(curr);
        }

        return result;
    }
};
```

## 876. Middle of the Linked List
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
    ListNode* middleNode(ListNode* head) {
        auto slow = head;
        auto fast = head;

        while (fast && fast->next) {
            fast = fast->next->next;
            slow = slow->next;
        }

        return slow;
    }
};
```

## 1742. Maximum Number of Balls in a Box
```cpp
class Solution {
public:
    int countBalls(int lowLimit, int highLimit) {
        vector<int> buckets(46, 0);

        int maxBallCount = 0;
        for (int num = lowLimit; num <= highLimit; num++) {
            int digitSum = getDigitSum(num);
            buckets[digitSum]++;

            maxBallCount = max(maxBallCount, buckets[digitSum]);
        }

        return maxBallCount;
    }

private:
    int getDigitSum(int num) {
        int sum = 0;

        while (num) {
            int digit = num % 10;
            sum += digit;
            num /= 10;
        }

        return sum;
    }
};
```

## 1880. Check if Word Equals Summation of Two Words
```cpp
class Solution {
public:
    bool isSumEqual(string firstWord, string secondWord, string targetWord) {
        return strToNum(firstWord) + strToNum(secondWord) == strToNum(targetWord);
    }

private:
    int strToNum(string& s) {
        int sum = 0;

        for (const char& c : s) {
            sum *= 10;
            sum += c - 'a';
        }

        return sum;
    }
};
```

## 1022. Sum of Root To Leaf Binary Numbers
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
    int sumRootToLeaf(TreeNode* root, int bin = 0) {
        if (!root) {
            return 0;
        }

        int nextBin = (bin << 1) | root->val;

        if (!root->left && !root->right) {
            return nextBin;
        }
        
        int left = sumRootToLeaf(root->left, nextBin);
        int right = sumRootToLeaf(root->right, nextBin);

        return left + right;   
    }
};
```

## 1207. Unique Number of Occurrences
```cpp
class Solution {
public:
    bool uniqueOccurrences(vector<int>& arr) {
        vector<int> occurrences(2001, 0);
        for (const int num : arr) {
            occurrences[num + 1000]++;
        }

        set<int> unique;
        for (const int occurrence : occurrences) {
            if (occurrence == 0) {
                continue;
            }
            
            if (unique.count(occurrence) == 1) {
                return false;
            }

            unique.insert(occurrence);
        }

        return true;
    }
};
```

## 2225. Find Players With Zero or One Losses
```cpp
class Solution {
public:
    vector<vector<int>> findWinners(vector<vector<int>>& matches) {
        unordered_map<int, int> playerToLosses;
        for (const auto& match : matches) {
            playerToLosses[match[0]];
            playerToLosses[match[1]]++;
        }

        vector<int> zeroLosses;
        vector<int> oneLoss;

        for (const auto& [player, losses] : playerToLosses) {
            if (losses == 0) {
                zeroLosses.push_back(player);
            }

            if (losses == 1) {
                oneLoss.push_back(player);
            }
        }

        sort(zeroLosses.begin(), zeroLosses.end());
        sort(oneLoss.begin(), oneLoss.end());

        return {zeroLosses, oneLoss};
    }
};
```

## 784. Letter Case Permutation
### Iterative Bit Manipulation
```cpp
class Solution {
public:
    vector<string> letterCasePermutation(string s) {
        vector<string> result;

        vector<int> alphaIndices;
        for (int i = 0; i < s.size(); i++) {
            if (isalpha(s[i])) {
                alphaIndices.push_back(i);
            }
        }

        int upperBound = (1 << alphaIndices.size()) - 1;
        for (int bin = 0; bin <= upperBound; bin++) {
            string curr = s;

            for (int i = 0; i < alphaIndices.size(); i++) {
                if ((1 << i) & bin) {
                    curr[alphaIndices[i]] = toupper(curr[alphaIndices[i]]);
                } else {
                    curr[alphaIndices[i]] = tolower(curr[alphaIndices[i]]);
                }
            }

            result.push_back(curr);
        }

        return result;
    }
};
```

### Recursive
```cpp
class Solution {
public:
    vector<string> letterCasePermutation(string s) {
        vector<string> result;
        string curr = s;
        _letterCasePermutation(curr, 0, result);
        return result;
    }

private:
    void _letterCasePermutation(string& curr, int index, vector<string>& result) {
        if (index >= curr.size()) {
            result.push_back(curr);
            return;
        }

        if (isalpha(curr[index])) {
            curr[index] = toupper(curr[index]);
            _letterCasePermutation(curr, index + 1, result);

            curr[index] = tolower(curr[index]);
            _letterCasePermutation(curr, index + 1, result);
        } 
        // digit
        else {
            _letterCasePermutation(curr, index + 1, result);
        }
    }
};
```

## 2283. Check if Number Has Equal Digit Count and Digit Value
```cpp
class Solution {
public:
    bool digitCount(string num) {
        vector<int> counts(10, 0);
        for (int i = 0; i < num.size(); i++) {
            int digit = num[i] - '0';
            counts[digit]++;
        }

        for (int digit = 0; digit < num.size(); digit++) {
            if (counts[digit] != num[digit] - '0') {
                return false;
            }
        }

        return true;
    }
};
```

## 2255. Count Prefixes of a Given String
```cpp
class Solution {
public:
    int countPrefixes(vector<string>& words, string s) {
        int count = 0;
        
        for (const auto& word : words) {
            count += isPrefix(word, s);
        }

        return count;
    }

private:
    bool isPrefix(const string& word, const string& s) const {
        if (word.size() > s.size()) {
            return false;
        }

        for (int i = 0; i < word.size(); i++) {
            if (word[i] != s[i]) {
                return false;
            }
        }

        return true;
    }
};
```

## 94. Binary Tree Inorder Traversal
### Recursive
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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        _inorderTraversal(root, result);
        return result;
    }

private:
    void _inorderTraversal(TreeNode* root, vector<int>& result) {
        if (!root) {
            return;
        }

        _inorderTraversal(root->left, result);
        result.push_back(root->val);
        _inorderTraversal(root->right, result);
    }
};
```

### Iterative
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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;

        vector<TreeNode*> stack;
        TreeNode* curr = root;

        while (curr || stack.size()) {
            while (curr) {
                stack.push_back(curr);
                curr = curr->left;
            }

            auto node = stack.back();
            stack.pop_back();

            result.push_back(node->val);

            curr = node->right;
        }

        return result;
    }
};
```

### Morris
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
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> result;
        TreeNode* curr = root;

        while (curr) {
            if (!curr->left) {
                result.push_back(curr->val);
                curr = curr->right;
                continue;
            }

            TreeNode* pred = curr->left;
            while (pred && pred->right && pred->right != curr) {
                pred = pred->right;
            }

            if (pred->right) {
                result.push_back(curr->val);
                
                pred->right = nullptr;
                curr = curr->right;
            } else {
                pred->right = curr;
                curr = curr->left;
            }
        }

        return result;
    }
};
```

## 2154. Keep Multiplying Found Values by Two
```cpp
class Solution {
public:
    int findFinalValue(vector<int>& nums, int original) {
        vector<int> buckets(1001, 0);
        for (const int num : nums) {
            buckets[num]++;
        }

        while (original < buckets.size() && buckets[original]) {
            original *= 2;
        }

        return original;
    }
};
```

## 814. Binary Tree Pruning
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
    TreeNode* pruneTree(TreeNode* root) {
        if (!root) {
            return nullptr;
        }
        
        root->left = pruneTree(root->left);
        root->right = pruneTree(root->right);

        if (!root->left && !root->right && root->val != 1) {
            return nullptr;
        }

        return root;  
    }
};
```

## 1529. Minimum Suffix Flips
```cpp
class Solution {
public:
    int minFlips(string target) {
        int flips = 0;

        char prev = '0';
        for (int i = 0; i < target.size(); i++) {
            if (target[i] != prev) {
                flips++;
            }

            prev = target[i];
        }

        return flips;
    }
};
```

## 1310. XOR Queries of a Subarray
```cpp
class Solution {
public:
    vector<int> xorQueries(vector<int>& arr, vector<vector<int>>& queries) {
        vector<int> xorPrefix{0};
        for (int i = 0; i < arr.size(); i++) {
            xorPrefix.push_back(xorPrefix.back() ^ arr[i]);
        }

        vector<int> result;
        for (const auto& query : queries) {
            auto start = query[0];
            auto end = query[1];
            result.push_back(xorPrefix[end + 1] ^ xorPrefix[start]);
        }

        return result;
    }
};
```

## 1460. Make Two Arrays Equal by Reversing Subarrays
```cpp
class Solution {
public:
    bool canBeEqual(vector<int>& target, vector<int>& arr) {
        if (target.size() != arr.size()) {
            return false;
        }

        vector<int> counts(1001, 0);
        for (int i = 0; i < target.size(); i++) {
            counts[target[i]]++;
            counts[arr[i]]--;
        }

        for (const auto c : counts) {
            if (c != 0) {
                return false;
            }
        }

        return true;
    }
};
```

## 1403. Minimum Subsequence in Non-Increasing Order
```cpp
class Solution {
public:
    vector<int> minSubsequence(vector<int>& nums) {
        int totalSum = 0;
        vector<int> counts(101, 0);
        for (const int num : nums) {
            totalSum += num;
            counts[num]++;
        }

        vector<int> result;
        int currSum = 0;
        for (int num = counts.size() - 1; num >= 0; num--) {
            while (counts[num]--) {
                currSum += num;
                result.push_back(num);
                if (totalSum - currSum < currSum) {
                    return result;
                }
            }
        }

        return result;
    }
};
```

## 2186. Minimum Number of Steps to Make Two Strings Anagram II
```cpp
class Solution {
public:
    int minSteps(string s, string t) {
        vector<int> counts(26, 0);
        for (const auto c : s) {
            counts[c - 'a']++;
        }

        for (const auto c : t) {
            counts[c - 'a']--;
        }

        int steps = 0;
        for (int i = 0; i < counts.size(); i++) {
            steps += abs(counts[i]);
        }
        return steps;
    }
};
```

## 1356. Sort Integers by The Number of 1 Bits
```cpp
class Solution {
public:
    vector<int> sortByBits(vector<int>& arr) {
        sort(arr.begin(), arr.end(), [](const auto a, const auto b) {
            auto aCount = __builtin_popcount(a);
            auto bCount = __builtin_popcount(b);

            if (aCount == bCount) {
                return a < b;
            }

            return aCount < bCount;
        });

        return arr;
    }
};
```

## 22. Generate Parentheses
```cpp
class Solution {
public:
    vector<string> generateParenthesis(int n) {
        vector<string> result;
        string curr;
        _generateParenthesis(result, curr, n, n);
        return result;
    }

private:
    void _generateParenthesis(vector<string>& result, string& curr, int open, int close) {
        if (open > close || open < 0 || close < 0) {
            return;
        }
        
        if (open == 0 && close == 0) {
            result.push_back(curr);
            return;
        }

        // open
        curr.push_back('(');
        _generateParenthesis(result, curr, open - 1, close);
        curr.pop_back();

        // close
        curr.push_back(')');
        _generateParenthesis(result, curr, open, close - 1);
        curr.pop_back();
    }
};
```

## 977. Squares of a Sorted Array
```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        vector<int> squares(nums.size());
        
        int left = 0;
        int right = nums.size() - 1;
        for (int i = squares.size() - 1; i >= 0; i--) {
            int leftSquared = pow(nums[left], 2);
            int rightSquared = pow(nums[right], 2);

            if (leftSquared < rightSquared) {
                squares[i] = rightSquared;
                right--;
            } else {
                squares[i] = leftSquared;
                left++;
            }
        }

        return squares;
    }
};
```

## 2053. Kth Distinct String in an Array
```cpp
class Solution {
public:
    string kthDistinct(vector<string>& arr, int k) {
        unordered_map<string, int> seen;
        for (const auto& s : arr) {
            seen[s]++;
        }

        int distinctCount = 0;
        for (const auto& s : arr) {
            if (seen[s] == 1) {
                distinctCount++;
            }

            if (distinctCount == k) {
                return s;
            }
        }

        return "";
    }
};
```

## 2124. Check if All A's Appears Before All B's
```cpp
class Solution {
public:
    bool checkString(string s) {
        bool bSeen = false;
        for (const auto c : s) {
            if (c == 'b') {
                bSeen = true;
            }

            if (c == 'a' && bSeen) {
                return false;
            }
        }

        return true;
    }
};
```

## 1974. Minimum Time to Type Word Using Special Typewriter
```cpp
class Solution {
public:
    int minTimeToType(string word) {
        int seconds = 0;
        int pointer = 'a';
        for (const auto c : word) {
            int a = abs((c - 'a') - (pointer - 'a'));
            int b = 26 - a;

            seconds += min(a, b) + 1;

            pointer = c;
        }

        return seconds;
    }
};
```

## 496. Next Greater Element I
```cpp
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        unordered_map<int, int> greaterMap;
        vector<int> stack;

        for (const auto num : nums2) {
            while (stack.size() && stack.back() < num) {
                greaterMap[stack.back()] = num;
                stack.pop_back();
            }

            stack.push_back(num);
        }

        vector<int> result(nums1.size(), -1);
        for (int i = 0; i < nums1.size(); i++) {
            if (greaterMap.count(nums1[i]) == 0) {
                result[i] = -1;
            } else {
                result[i] = greaterMap[nums1[i]];
            }
        }

        return result;
    }
};
```

## 821. Shortest Distance to a Character
```cpp
class Solution {
public:
    vector<int> shortestToChar(string s, char c) {
        vector<int> result(s.size());

        int leftIndex = INT_MAX;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == c) {
                leftIndex = i;
            }

            result[i] = abs(i - leftIndex);
        }

        int rightIndex = INT_MAX;
        for (int i = s.size() - 1; i >= 0; i--) {
            if (s[i] == c) {
                rightIndex = i;
            }

            result[i] = min(result[i], abs(rightIndex - i));
        }

        return result;
    }
};
```

## 986. Interval List Intersections
```cpp
class Solution {
public:
    vector<vector<int>> intervalIntersection(vector<vector<int>>& firstList, vector<vector<int>>& secondList) {
        vector<vector<int>> result;
        int first = 0;
        int second = 0;

        while (first < firstList.size() && second < secondList.size()) {
            const int firstStart = firstList[first][0];
            const int firstEnd = firstList[first][1];

            const int secondStart = secondList[second][0];
            const int secondEnd = secondList[second][1];
            
            const int overlapStart = max(firstStart, secondStart);
            const int overlapEnd = min(firstEnd, secondEnd);

            if (overlapStart <= overlapEnd) {
                result.push_back({overlapStart, overlapEnd});
            }

            if (firstEnd == overlapEnd) {
                first++;
            } else {
                second++;
            }
        }

        return result;
    }
};
```

## 2057. Smallest Index With Equal Value
```cpp
class Solution {
public:
    int smallestEqual(vector<int>& nums) {
        for (int i = 0; i < nums.size(); i++) {
            if (i % 10 == nums[i]) {
                return i;
            }
        }

        return -1;
    }
};
```

## 2352. Equal Row and Column Pairs
```cpp
class Solution {
public:
    int equalPairs(vector<vector<int>>& grid) {
        unordered_map<string, int> m;

        for (int row = 0; row < grid.size(); row++) {
            string r;
            for (int col = 0; col < grid[0].size(); col++) {
                r += to_string(grid[row][col]);
                r += '-';
            }
            m[r]++;
        }

        int count = 0;
        for (int col = 0; col < grid[0].size(); col++) {
            string c;
            for (int row = 0; row < grid.size(); row++) {
                c += to_string(grid[row][col]);
                c += '-';
            }
            count += m[c];
        }

        return count;
    }
};
```

## 2487. Remove Nodes From Linked List
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
    ListNode* removeNodes(ListNode* head) {
        ListNode* dummy = new ListNode(INT_MAX);
        dummy->next = head;

        vector<ListNode*> stack { dummy };
        
        for (ListNode* curr = head; curr; curr = curr->next) {
            while (stack.back()->val < curr->val) {
                ListNode* node = stack.back();
                delete node;
                stack.pop_back();
            }

            stack.back()->next = curr;

            stack.push_back(curr);
        }

        return dummy->next;
    }
};
```

## 893. Groups of Special-Equivalent Strings
```cpp
class Solution {
public:
    int numSpecialEquivGroups(vector<string>& words) {
        unordered_set<string> counts;

        for (const auto& word : words) {
            string group = getGroup(word);
            counts.insert(group);
        }

        return counts.size();
    }

private:
    string getGroup(string word) {
        vector<int> oddCounts(26, 0);
        vector<int> evenCounts(26, 0);
        for (int i = 0; i < word.size(); i++) {
            if (i & 1) {
                oddCounts[word[i] - 'a']++;
            } else {
                evenCounts[word[i] - 'a']++;
            }
        }

        string group;
        for (int i = 0; i < oddCounts.size(); i++) {
            group += string(oddCounts[i], i + 'a');
        }

        for (int i = 0; i < evenCounts.size(); i++) {
            group += string(evenCounts[i], i + 'a');
        }

        return group;
    }
};
```

## 883. Projection Area of 3D Shapes
```cpp
class Solution {
public:
    int projectionArea(vector<vector<int>>& grid) {
        int area = 0;
        for (int row = 0; row < grid.size(); row++) {
            int maxRow = 0;
            int maxCol = 0;

            for (int col = 0; col < grid[0].size(); col++) {
                if (grid[row][col] != 0) {
                    area++;
                }

                maxRow = max(maxRow, grid[row][col]);
                maxCol = max(maxCol, grid[col][row]);
            }

            area += maxRow + maxCol;
        }

        return area;
    }
};
```

## 1380. Lucky Numbers in a Matrix
```cpp
class Solution {
public:
    vector<int> luckyNumbers (vector<vector<int>>& matrix) {
        vector<int> minRows(matrix.size(), INT_MAX);
        vector<int> maxCols(matrix[0].size(), INT_MIN);

        for (int row = 0; row < matrix.size(); row++) {
            for (int col = 0; col < matrix[0].size(); col++) {
                minRows[row] = min(minRows[row], matrix[row][col]);
                maxCols[col] = max(maxCols[col], matrix[row][col]);
            }
        }

        vector<int> result;
        for (int i = 0; i < minRows.size(); i++) {
            for (int j = 0; j < maxCols.size(); j++) {
                if (minRows[i] == maxCols[j]) {
                    result.push_back(minRows[i]);
                }
            }
        }
        return result;
    }
};
```

## 349. Intersection of Two Arrays
```cpp
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> s1(nums1.begin(), nums1.end());
        unordered_set<int> result;

        for (const int num : nums2) {
            if (s1.count(num) == 0) {
                continue;
            }

            result.insert(num);
        }

        return vector(result.begin(), result.end());
    }
};
```

## 2399. Check Distances Between Same Letters
```cpp
class Solution {
public:
    bool checkDistances(string s, vector<int>& distance) {
        vector<int> firstMap(26, -1);
        for (int i = 0; i < s.size(); i++) {
            if (firstMap[s[i] - 'a'] == -1) {
                firstMap[s[i] - 'a'] = i;
                continue;
            }

            int dist = i - firstMap[s[i] - 'a'] - 1;
            if (distance[s[i] - 'a'] != dist) {
                return false;
            }
        }

        return true;
    }
};
```

## 1876. Substrings of Size Three with Distinct Characters
```cpp
class Solution {
public:
    int countGoodSubstrings(string s, int k = 3) {
        int count = 0;

        vector<int> charCounts(26, 0);
        int left = 0;
        for (int right = 0; right < s.size(); right++) {
            charCounts[s[right] - 'a']++;

            if (right < k - 1) {
                continue;
            }

            while (right - left + 1 > k) {
                charCounts[s[left] - 'a']--;
                left++;
            }

            bool good = true;
            for (int i = 0; i < charCounts.size(); i++) {
                if (charCounts[i] > 1) {
                    good = false;
                    break;
                }
            }

            count += good;
        }

        return count;
    }
};
```

## 1047. Remove All Adjacent Duplicates In String
```cpp
class Solution {
public:
    string removeDuplicates(string s) {
        vector<char> stack;
        for (const char c : s) {
            if (stack.size() && stack.back() == c) {
                stack.pop_back();
                continue;
            }

            stack.push_back(c);
        }

        return string(stack.begin(), stack.end());
    }
};
```

## 136. Single Number
```cpp
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int x = 0;
        
        for (const int num : nums) {
            x ^= num;
        }

        return x;
    }
};
```

## 969. Pancake Sorting
```cpp
class Solution {
public:
    vector<int> pancakeSort(vector<int>& arr) {
        vector<int> result;
        for (int finalI = arr.size() - 1; finalI >= 0; finalI--) {
            // find largest that has not yet been placed
            int largest = 0;
            for (int i = 0; i <= finalI; i++) {
                if (arr[largest] < arr[i]) {
                    largest = i;
                }
            }

            result.push_back(largest + 1);
            result.push_back(finalI + 1);
            
            reverse(arr.begin(), arr.begin() + largest + 1);
            reverse(arr.begin(), arr.begin() + finalI + 1);
        }

        return result;
    }
};
```

## 2085. Count Common Words With One Occurrence
```cpp
class Solution {
public:
    int countWords(vector<string>& words1, vector<string>& words2) {
        unordered_map<string, int> w1Counts;
        for (const string& word : words1) {
            w1Counts[word]++;
        }

        unordered_map<string, int> w2Counts;
        for (const string& word : words2) {
            w2Counts[word]++;
        }

        int count = 0;
        for (const auto [key, value] : w1Counts) {
            if (value == 1 && w2Counts[key] == 1) {
                count++;
            }
        }

        return count;
    }
};
```

## 1967. Number of Strings That Appear as Substrings in Word
```cpp
class Solution {
public:
    int numOfStrings(vector<string>& patterns, string word) {
        int count = 0;

        for (const auto pattern : patterns) {
            if (word.find(pattern) != string::npos) {
                count++;
            }
        }

        return count;
    }
};
```

## 1338. Reduce Array Size to The Half
```cpp
class Solution {
public:
    int minSetSize(vector<int>& arr) {
        unordered_map<int, int> counts;
        for (const int a : arr) {
            counts[a]++;
        }

        vector<pair<int, int>> sortedCounts(counts.begin(), counts.end());
        sort(sortedCounts.begin(), sortedCounts.end(), [](const auto& a, const auto& b) {
            return a.second > b.second;
        });

        int half = arr.size() / 2;
        int setSize = 0;
        for (const auto [key, value] : sortedCounts) {
            half -= value;
            setSize++;

            if (half <= 0) {
                return setSize;
            }
        }

        return setSize;
    }
};
```

## 944. Delete Columns to Make Sorted
```cpp
class Solution {
public:
    int minDeletionSize(vector<string>& strs) {
        int unsorted = 0;
        for (int col = 0; col < strs[0].size(); col++) {
            for (int row = 0; row < strs.size() - 1; row++) {
                if (strs[row][col] > strs[row + 1][col]) {
                    unsorted++;
                    break;
                }
            }
        }
        return unsorted;
    }
};
```

## 451. Sort Characters By Frequency
```cpp
class Solution {
public:
    string frequencySort(string s) {
        unordered_map<char, int> m;
        for (const auto c : s) {
            m[c]++;
        }

        vector<pair<char, int>> pairs(m.begin(), m.end());
        sort(pairs.begin(), pairs.end(), [](const auto a, const auto b) {
            return a.second > b.second;
        });

        string result;
        for (const auto [key, value] : pairs) {
            result += string(value, key);
        }
        return result;
    }
};
```

## 852. Peak Index in a Mountain Array
```cpp
class Solution {
public:
    int peakIndexInMountainArray(vector<int>& arr) {
        // peak can't be first index (index: 0)
        int left = 1; 
        // peak can't be last index (index: arr.size() - 1)
        int right = arr.size() - 2;

        while (left < right) {
            int mid = (right - left) / 2 + left;

            if (arr[mid] < arr[mid + 1]) {
                left = mid + 1;
            } else {
                right = mid;
            }
        }

        return left;
    }
};
```

## 2248. Intersection of Multiple Arrays
```cpp
class Solution {
public:
    vector<int> intersection(vector<vector<int>>& nums) {
        vector<int> counts(1001, 0);

        for (const auto arr : nums) {
            for (const int num : arr) {
                counts[num]++;
            }
        }

        vector<int> result;
        for (int num = 0; num < counts.size(); num++) {
            if (counts[num] != nums.size()) {
                continue;
            }

            result.push_back(num);
        }

        return result;
    }
};
```

## 1525. Number of Good Ways to Split a String
```cpp
class Solution {
public:
    int numSplits(string s) {
        vector<int> left(26, 0);
        vector<int> right(26, 0);

        for (const char c : s) {
            right[c - 'a']++;
        }

        int count = 0;
        for (int i = 0; i < s.size(); i++) {
            left[s[i] - 'a']++;
            right[s[i] - 'a']--;

            count += distinctCount(left) == distinctCount(right);
        }

        return count;
    }

private:
    int distinctCount(vector<int>& arr) {
        int count = 0;

        for (const int a : arr) {
            if (a != 0) {
                count += 1;
            }
        }

        return count;
    }
};
```

## 2215. Find the Difference of Two Arrays
```cpp
class Solution {
public:
    vector<vector<int>> findDifference(vector<int>& nums1, vector<int>& nums2) {
        unordered_set<int> set1(nums1.begin(), nums1.end());
        unordered_set<int> set2(nums2.begin(), nums2.end());
        
        vector<int> result1;
        for (const auto num : set1) {
            if (set2.count(num) == 0) {
                result1.push_back(num);
            }
        }

        vector<int> result2;
        for (const auto num : set2) {
            if (set1.count(num) == 0) {
                result2.push_back(num);
            }
        }

        return {result1, result2};
    }
};
```

## 509. Fibonacci Number
### Recursive
```cpp
class Solution {
public:
    Solution() : memo(vector(31, INT_MIN)) {}

    int fib(int n) {
        if (n <= 1) {
            return n;
        }

        if (memo[n] != INT_MIN) {
            return memo[n];
        }


        memo[n] = fib(n - 1) + fib(n - 2);
        return memo[n];
    }

private:
    vector<int> memo;
};
```

### DP
```cpp
class Solution {
public:
    int fib(int n) {
        if (n <= 1) {
            return n;
        }

        vector<int> dp(n + 1, 0);
        dp[0] = 0;
        dp[1] = 1;
        
        for (int i = 2; i <= n; i++) {
            dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[n];
    }
};
```

## 108. Convert Sorted Array to Binary Search Tree
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
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        return _sortedArrayToBST(nums, 0, nums.size() - 1);
    }

private:
    TreeNode* _sortedArrayToBST(vector<int>& nums, int left, int right) {
        if (left > right) {
            return nullptr;
        }

        int mid = (right - left) / 2 + left;
        auto node = new TreeNode(nums[mid]);
        node->left = _sortedArrayToBST(nums, left, mid - 1);
        node->right = _sortedArrayToBST(nums, mid + 1, right);
        return node;
    }
};
```

## 1433. Check If a String Can Break Another String
```cpp
class Solution {
public:
    bool checkIfCanBreak(string s1, string s2) {
        sortStr(s1);
        sortStr(s2);
        return canBreak(s1, s2) || canBreak(s2, s1);
    }

private:
    bool canBreak(string& s1, string s2) {
        for (int i = 0; i < s1.size(); i++) {
            if (s1[i] < s2[i]) {
                return false;
            }
        }

        return true;
    }

    void sortStr(string& s) {
        vector<int> count(26, 0);
        for (const char c : s) {
            count[c - 'a']++;
        }

        s = "";
        for (int i = 0; i < count.size(); i++) {
            s += string(count[i], 'a' + i);
        }
    }
};
```

## 1636. Sort Array by Increasing Frequency
```cpp
class Solution {
public:
    vector<int> frequencySort(vector<int>& nums) {
        vector<int> counts(201, 0);
        for (const int num : nums) {
            counts[num + 100]++;
        }

        vector<vector<int>> freq(201);
        for (int i = 0; i < counts.size(); i++) {
            int count = counts[i];
            while (counts[i]--) {
                freq[count].push_back(i);
            }
        }

        vector<int> result;
        for (int i = 0; i < freq.size(); i++) {
            while (freq[i].size() > 0) {
                result.push_back(freq[i].back() - 100);
                freq[i].pop_back();
            }
        }
        return result;
    }
};
```

## 2309. Greatest English Letter in Upper and Lower Case
```cpp
class Solution {
public:
    string greatestLetter(string s) {
        vector<int> lowerCounts(26, 0);
        vector<int> upperCounts(26, 0);

        for (const char c : s) {
            if (c == tolower(c)) {
                lowerCounts[c - 'a']++;
            } else {
                upperCounts[c - 'A']++;
            }
        }

        for (int i = lowerCounts.size() - 1; i >= 0; i--) {
            if (lowerCounts[i] != 0 && upperCounts[i] != 0) {
                return string(1, i + 'A');
            }
        }

        return "";
    }
};
```

## 1002. Find Common Characters
```cpp
class Solution {
public:
    vector<string> commonChars(vector<string>& words) {
        vector<int> count1 = getCounts(words[0]);
        for (int i = 1; i < words.size(); i++) {
            vector<int> count2 = getCounts(words[i]);
            getCommon(count1, count2);
        }

        vector<string> result;
        for (int i = 0; i < count1.size(); i++) {
            while (count1[i]--) {
                result.push_back(string(1, i + 'a'));
            }
        }

        return result;
    }

private:
    void getCommon(vector<int>& count1, vector<int>& count2) {
        for (int i = 0; i < count1.size(); i++) {
            count1[i] = min(count1[i], count2[i]);
        }
    }

    vector<int> getCounts(string& word) {
        vector<int> counts(26, 0);

        for (const char c : word) {
            counts[c - 'a']++;
        }

        return counts;
    }
};
```

## 985. Sum of Even Numbers After Queries
```cpp
class Solution {
public:
    vector<int> sumEvenAfterQueries(vector<int>& nums, vector<vector<int>>& queries) {
        int sum = 0;
        for (const int num : nums) {
            if (num % 2 == 0) {
                sum += num;
            }
        }

        vector<int> result;
        for (const auto query : queries) {
            const int value = query[0];
            const int index = query[1];

            if (nums[index] % 2 == 0) {
                sum -= nums[index];
            }

            nums[index] += value;
            
            if (nums[index] % 2 == 0) {
                sum += nums[index];
            }

            result.push_back(sum);
        }

        return result;
    }
};
```

## 1413. Minimum Value to Get Positive Step by Step Sum
```cpp
class Solution {
public:
    int minStartValue(vector<int>& nums) {
        int min = 0;
        int sum = 0;
        for (const auto num : nums) {
            sum += num;
            min = std::min(sum, min);
        }
        return -min + 1;
    }
};
```

## 999. Available Captures for Rook
```cpp
class Solution {
public:
    int numRookCaptures(vector<vector<char>>& board) {
        int rookRow = 0;
        int rookCol = 0;

        for (int row = 0; row < board.size(); row++) {
            for (int col = 0; col < board[0].size(); col++) {
                if (board[row][col] == 'R') {
                    rookRow = row;
                    rookCol = col;

                    goto end;
                }
            }
        }
        end:


        int capture = 0;

        // up
        for (int row = rookRow; row >= 0; row--) {
            if (board[row][rookCol] == 'B') {
                break;
            }

            if (board[row][rookCol] == 'p') {
                capture++;
                break;
            }
        }

        // down
        for (int row = rookRow; row < board.size(); row++) {
            if (board[row][rookCol] == 'B') {
                break;
            }

            if (board[row][rookCol] == 'p') {
                capture++;
                break;
            }
        }

        // left
        for (int col = rookCol; col >= 0; col--) {
            if (board[rookRow][col] == 'B') {
                break;
            }

            if (board[rookRow][col] == 'p') {
                capture++;
                break;
            }
        }

        // right
        for (int col = rookCol; col < board[0].size(); col++) {
            if (board[rookRow][col] == 'B') {
                break;
            }

            if (board[rookRow][col] == 'p') {
                capture++;
                break;
            }
        }

        return capture;
    }
};
```

## 39. Combination Sum
```cpp
class Solution {
public:
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        vector<vector<int>> result;
        vector<int> curr;
        _combinationSum(result, candidates, curr, 0, target, 0);
        return result;   
    }

private:
    void _combinationSum(vector<vector<int>>& result, vector<int>& candidates, vector<int>& curr, int index, int target, int sum) {
        if (index >= candidates.size()) {
            return;
        }

        if (sum > target) {
            return;
        }

        if (sum == target) {
            result.push_back(curr);
            return;
        }

        // take
        curr.push_back(candidates[index]);
        _combinationSum(result, candidates, curr, index, target, sum + candidates[index]);
        curr.pop_back();

        // don't take
        _combinationSum(result, candidates, curr, index + 1, target, sum);
    }
};
```

## 1700. Number of Students Unable to Eat Lunch
```cpp
class Solution {
public:
    int countStudents(vector<int>& students, vector<int>& sandwiches) {
        int zeroCount = 0;
        int oneCount = 0;

        for (const int student : students) {
            student == 0 ? zeroCount++ : oneCount++;
        }

        for (const int sandwich : sandwiches) {
            if (sandwich == 0 && zeroCount > 0) {
                zeroCount--;
                continue;
            }

            if (sandwich == 1 && oneCount > 0) {
                oneCount--;
                continue;
            }

            break;
        }

        return zeroCount + oneCount;
    }
};
```

## 824. Goat Latin
```cpp
class Solution {
public:
    string toGoatLatin(string sentence) {
        string result;

        vector<string> words = splitWords(sentence);
        for (int i = 0; i < words.size(); i++) { 
            const string& word = words[i];
            if (isVowel(word[0])) {
                result += word;
                result += "ma";
            } else {
                result += word.substr(1);
                result += word[0];
                result += "ma";
            }

            result += string(i + 1, 'a');
            result += " ";
        }

        result.pop_back();
        return result;
    }

private:
    const unordered_set<char> vowels {'a','e','i','o','u'};

    bool isVowel(const char c) {
        return vowels.count(tolower(c));
    }

    vector<string> splitWords(string sentence) {
        vector<string> result;
        string curr;
        for (int i = 0; i <= sentence.size(); i++) {
            if (i >= sentence.size() || sentence[i] == ' ') {
                result.push_back(curr);
                curr = "";
                continue;
            }

            curr += sentence[i];
        }

        return result;
    }
};
```

## 2441. Largest Positive Integer That Exists With Its Negative
```cpp
class Solution {
public:
    int findMaxK(vector<int>& nums) {
       unordered_set<int> seen;
       int maxNum = 0;
       for (const int num : nums) {
           if (seen.count(-1 * num)) {
               maxNum = max(maxNum, abs(num));
           }

           seen.insert(num);
       }

       return maxNum == 0 ? -1 : maxNum;
    }
};
```

## 1160. Find Words That Can Be Formed by Characters
```cpp
class Solution {
public:
    int countCharacters(vector<string>& words, string chars) {
        const vector<int> counts { getLetterCounts(chars) };

        int count = 0;
        for (const string word : words) {
            if (isGood(word, counts)) {
                count += word.size();
            }
        }

        return count;
    }

private:
    bool isGood(const string& word, const vector<int>& counts) {
        const vector<int> wordCounts { getLetterCounts(word) };
        
        for (int i = 0; i < wordCounts.size(); i++) {
            if (counts[i] < wordCounts[i]) {
                return false;
            }
        }

        return true;
    }

    vector<int> getLetterCounts(const string& word) {
        vector<int> counts(26, 0);
        for (const char c : word) {
            counts[c - 'a']++;
        }

        return counts;
    }
};
```

## 1721. Swapping Nodes in a Linked List
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
    ListNode* swapNodes(ListNode* head, int k) {
        ListNode* kFromStart { head };
        for (int i = 1; i < k; i++) {
            kFromStart = kFromStart->next;
        }

        ListNode* fast { kFromStart };
        ListNode* kFromEnd { head };
        while (fast && fast->next) {
            fast = fast->next;
            kFromEnd = kFromEnd->next;
        }

        swap(kFromStart->val, kFromEnd->val);
        return head;
    }
};
```

## 1343. Number of Sub-arrays of Size K and Average Greater than or Equal to Threshold
```cpp
class Solution {
public:
    int numOfSubarrays(vector<int>& arr, int k, int threshold) {
        int sum = 0;
        int count = 0;

        int left = 0;
        for (int right = 0; right < arr.size(); right++) {
            sum += arr[right];

            if (right < k - 1) {
                continue;
            }

            int avg = sum / k;
            if (avg >= threshold) {
                count++;
            }

            sum -= arr[left];
            left++;
        }

        return count;
    }
};
```

## 538. Convert BST to Greater Tree
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
    TreeNode* convertBST(TreeNode* root) {
        int sum = 0;
        inOrderReverse(root, sum);
        return root;
    }

private:
    void inOrderReverse(TreeNode* root, int& sum) {
        if (!root) {
            return;
        }

        inOrderReverse(root->right, sum);
        
        sum += root->val;
        root->val = sum;

        inOrderReverse(root->left, sum);
    }
};
```

## 2319. Check if Matrix Is X-Matrix
```cpp
class Solution {
public:
    bool checkXMatrix(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size();

        for (int row = 0; row < m; row++) {
            for (int col = 0; col < n; col++) {
                if (row == col || col == n - row - 1) {
                    if (grid[row][col] == 0) {
                        return false;
                    }
                    continue;
                }

                if (grid[row][col] != 0) {
                    return false;
                }
            }
        }
        
        return true;
    }
};
```

## 1779. Find Nearest Point That Has the Same X or Y Coordinate
```cpp
class Solution {
public:
    int nearestValidPoint(int x, int y, vector<vector<int>>& points) {
        int minX = INT_MAX;
        int minY = INT_MAX;
        int minDist = INT_MAX;
        int minIndex = -1;

        for (int i = 0; i < points.size(); i++) {
            int currX = points[i][0];
            int currY = points[i][1];

            if (x == currX || y == currY) {
                int currDist = abs(x - currX) + abs(y - currY);
                if (currDist < minDist) {
                    minX = currX;
                    minY = currY;
                    minDist = currDist;
                    minIndex = i;
                }
            }
        }

        return minIndex;
    }
};
```

## 1991. Find the Middle Index in Array
```cpp
class Solution {
public:
    int findMiddleIndex(vector<int>& nums) {
        int rightSum = std::accumulate(nums.begin(), nums.end(), 0);
        int leftSum = 0;
        for (auto i = 0; i < nums.size(); ++i) {
            rightSum -= nums[i];

            if (leftSum == rightSum) {
                return i;
            }
            
            leftSum += nums[i];
        }

        return -1;
    }
};
```

## 1399. Count Largest Group
```cpp
class Solution {
public:
    int countLargestGroup(int n) {
        vector<int> dp(n + 1, 0);
        for (int num = 1; num <= n; num++) {
            dp[num] = num % 10 + dp[num / 10];
        }

        vector<int> groups(37, 0);
        int maxGroupSize = 0;
        for (int num = 1; num <= n; num++) {
            int sum = dp[num];
            groups[sum]++;
            maxGroupSize = max(maxGroupSize, groups[sum]);
        }

        int count = 0;
        for (const int group : groups) {
            if (group == maxGroupSize) {
                count++;
            }
        }

        return count;
    }
};
```

## 216. Combination Sum III
```cpp
class Solution {
public:
    vector<vector<int>> combinationSum3(int k, int n) {
        vector<vector<int>> result;
        vector<int> curr;
        _combinationSum3(result, curr, k, n, 1);
        return result;
    }

private:
    void _combinationSum3(vector<vector<int>>& result, vector<int>& curr, int digits, int sum, int currDigit) { 
        if (digits == 0 && sum == 0) {
            result.push_back(curr);
            return;
        }

        if (currDigit > 9 || digits <= 0 || sum <= 0) {
            return;
        }

        // take
        curr.push_back(currDigit);
        _combinationSum3(result, curr, digits - 1, sum - currDigit, currDigit + 1);
        curr.pop_back();

        // don't take
        _combinationSum3(result, curr, digits, sum, currDigit + 1);
    }
};
```

## 929. Unique Email Addresses
```cpp
class Solution {
public:
    int numUniqueEmails(vector<string>& emails) {
        unordered_set<string> seen;

        for (const string email : emails) {
            string parsedEmail = parseEmail(email);
            seen.insert(parsedEmail);
        }

        return seen.size();
    }

private:
    string parseEmail(string email) {
        // find @ sign index
        int atSignIndex = 0;
        while (atSignIndex < email.size() && email[atSignIndex] != '@') {
            atSignIndex++;
        }

        // local name
        string localName;
        for (int i = 0; i < atSignIndex; i++) {
            if (email[i] == '.') continue;
            if (email[i] == '+') break;
            localName += email[i];
        }

        // domain name
        return localName + email.substr(atSignIndex);
    }
};
```

## 2460. Apply Operations to an Array
```cpp
class Solution {
public:
    vector<int> applyOperations(vector<int>& nums) {
        int j = 0;
        for (int i = 0; i < nums.size(); i++) {
            if (i + 1 < nums.size() && nums[i] == nums[i + 1]) {
                nums[i] *= 2;
                nums[i + 1] = 0;
            }

            if (nums[i] != 0) {
                swap(nums[i], nums[j]);
                j++;
            }
        }

        return nums;
    }
};
```

## 145. Binary Tree Postorder Traversal
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
    vector<int> postorderTraversal(TreeNode* root) {
        if (!root) return {};

        vector<int> result;
        vector<TreeNode*> stack;
        TreeNode* curr { root };

        while (curr || stack.size()) {
            while (curr) {
                stack.push_back(curr);
                curr = curr->left;
            }

            if (stack.back()->right) {
                curr = stack.back()->right;
                continue;
            }

            TreeNode* node = stack.back();
            result.push_back(node->val);
            stack.pop_back();

            while (stack.size() && stack.back()->right == node) {
                node = stack.back();
                result.push_back(node->val);
                stack.pop_back();
            }
        }

        return result;
    }
};
```

## 289. Game of Life
```cpp
class Solution {
public:
    void gameOfLife(vector<vector<int>>& board) {
        const vector<pair<int, int>> dirs {{1,0}, {0,1}, {-1,0}, {0,-1}, {-1,-1}, {-1,1}, {1,-1}, {1,1}};

        for (int row = 0; row < board.size(); row++) {
            for (int col = 0; col < board[0].size(); col++) {
                int aliveNeighbors = 0;
                for (const auto [deltaRow, deltaCol] : dirs) {
                    int neighborRow = row + deltaRow;
                    int neighborCol = col + deltaCol;

                    // out of bounds
                    if (neighborRow < 0 || neighborRow >= board.size() || neighborCol < 0 || neighborCol >= board[0].size()) {
                        continue;
                    }

                    if ((board[neighborRow][neighborCol] & 1) == 1) {
                        aliveNeighbors++;
                    }
                }

                // Any live cell with two or three live neighbors lives on to the next generation.
                if ((board[row][col] & 1) == 1 && aliveNeighbors == 2 || aliveNeighbors == 3) {
                    board[row][col] |= 1 << 1;
                    continue;
                }

                // Any dead cell with exactly three live neighbors becomes a live cell, as if by reproduction.
                if ((board[row][col] & 0) == 0 && aliveNeighbors == 3) {
                    board[row][col] |= 1 << 1;
                    continue;
                }
            }
        }

        for (int row = 0; row < board.size(); row++) {
            for (int col = 0; col < board[0].size(); col++) {
                board[row][col] >>= 1;
            }
        }
    }
};
```

## 59. Spiral Matrix II
```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> result(n, vector<int>(n, 0));

        int top = 0;
        int bottom = n - 1;
        int left = 0;
        int right = n - 1;

        int curr = 1;

        while (top <= bottom) {
            // top
            for (int col = left; col <= right; col++) {
                result[top][col] = curr++;
            }
            top++;

            // right
            for (int row = top; row <= bottom; row++) {
                result[row][right] = curr++;
            }
            right--;

            // bottom
            for (int col = right; col >= left; col--) {
                result[bottom][col] = curr++;
            }
            bottom--;

            // left
            for (int row = bottom; row >= top; row--) {
                result[row][left] = curr++;
            }
            left++;
        }

        return result;
    }
};
```

## 1807. Evaluate the Bracket Pairs of a String
```cpp
class Solution {
public:
    string evaluate(string s, vector<vector<string>>& knowledge) {
        unordered_map<string, string> m;
        for (const auto k : knowledge) {
            m[k[0]] = k[1];
        }

        string result;
        string key;
        State state = State::OUTSIDE_KEY;

        for (int i = 0; i < s.size(); i++) {
            if (state == State::OUTSIDE_KEY) {
                if (s[i] == '(') {
                    state = State::INSIDE_KEY;
                } else {
                    result += s[i];
                }
            }
            else if (state == State::INSIDE_KEY) {
                if (s[i] == ')') {
                    state = State::OUTSIDE_KEY;
                    result += m.count(key) ? m[key] : "?";
                    key = "";
                } else {
                    key += s[i];
                }
            }
        }

        return result;
    }

private:
    enum class State { INSIDE_KEY, OUTSIDE_KEY };
};
```

## 739. Daily Temperatures
```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        vector<int> result(temperatures.size(), 0);
        vector<int> stack;
        for (int i = 0; i < temperatures.size(); i++) {
            while (stack.size() && temperatures[stack.back()] < temperatures[i]) {
                int index = stack.back();
                result[index] = i - index;
                stack.pop_back();
            }

            stack.push_back(i);
        }

        return result;
    }
};
```

## 49. Group Anagrams
```cpp
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        unordered_map<string, vector<string>> m;

        for (const string str : strs) {
            string key = getKey(str);
            m[key].push_back(str);
        }

        vector<vector<string>> result;

        for (const auto [key, value] : m) {
            result.push_back(value);
        }

        return result;
    }

private:
    string getKey(string str) {
        vector<int> counts(26, 0);

        for (const char c : str) {
            counts[c - 'a']++;
        }

        string key;
        for (int i = 0; i < counts.size(); i++) {
            key += string(1, counts[i] + '0');
            key += "-";
        }
        return key;
    }
};
```

## 2164. Sort Even and Odd Indices Independently
```cpp
class Solution {
public:
    vector<int> sortEvenOdd(vector<int>& nums) {
        vector<int> evens(101, 0);
        vector<int> odds(101, 0);
        for (int i = 0; i < nums.size(); i++) {
            if (i % 2 == 0) {
                evens[nums[i]]++;
            } else {
                odds[nums[i]]++;
            }
        }

        vector<int> result(nums.size(), 0);

        int evenIndex = 0;
        int oddIndex = 100;
        for (int i = 0; i < result.size(); i++) {
            if (i % 2 == 0) {
                while (evens[evenIndex] == 0) {
                    evenIndex++;         
                }

                evens[evenIndex]--;
                result[i] = evenIndex;
            } else {
                while (odds[oddIndex] == 0) {
                    oddIndex--;         
                }

                odds[oddIndex]--;
                result[i] = oddIndex;
            }

        }

        return result;
    }
};
```

## 647. Palindromic Substrings
### DP
```cpp
// Time: O(n^2), Space O(n^2)
class Solution {
public:
    int countSubstrings(string s) {
        vector<vector<bool>> dp(s.size(), vector(s.size(), false));

        int count = 0;
        for (int length = 1; length <= s.size(); length++) {
            for (int left = 0; left + length - 1 < s.size(); left++) {
                int right = left + length - 1;
                
                if (s[left] == s[right]) {
                    if (length <= 2) {
                        dp[left][right] = true;    
                    } 
                    else {
                        dp[left][right] = dp[left + 1][right - 1];
                    }
                }
                
                if (dp[left][right]) {
                    count++;
                }
            }
        }

        return count;
    }
};
```

### Expand From Middle
```cpp
// Time: O(n^2), Space: O(1)
class Solution {
public:
    int countSubstrings(string s) {
        int count = 0;

        // Odd length palindromes
        for (int mid = 0; mid < s.size(); mid++) {
            count += countPalindromes(s, mid, mid);
        }

        // Even length palindromes
        for (int mid = 0; mid < s.size() - 1; mid++) {
            count += countPalindromes(s, mid, mid + 1);
        }

        return count;
    }

private:
    int countPalindromes(string& s, int left, int right) {
        int count = 0;

        while (left >= 0 && right < s.size() && s[left] == s[right]) {
            count++;
            left--;
            right++;
        }

        return count;
    }
};
```

## 669. Trim a Binary Search Tree
```cpp
// Time: O(n), Space: O(h)
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
    TreeNode* trimBST(TreeNode* root, int low, int high) {
        if (!root) {
            return nullptr;
        }
        
        if (root->val < low) {
            return trimBST(root->right, low, high);
        }

        if (root->val > high) {
            return trimBST(root->left, low, high);
        }

        root->left = trimBST(root->left, low, high);
        root->right = trimBST(root->right, low, high);
        return root;
    }
};
```

## 77. Combinations
### Recursive
```cpp
// Time: O(n!/(n!-k!)k! * k), Space: O(n!/(n!-k!)k! * k)
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> result;
        vector<int> curr;
        _combine(result, curr, n, k, 1);
        return result;
    }

private:
    void _combine(vector<vector<int>>& result, vector<int>& curr, int n, int k, int num) {
        if (curr.size() == k) {
            result.push_back(curr);
            return;
        }

        if (curr.size() > k || num > n) {
            return;
        }
        
        // take
        curr.push_back(num);
        _combine(result, curr, n, k, num + 1);
        curr.pop_back();

        // don't take
        _combine(result, curr, n, k, num + 1);
    }
};
```

## Iterative
```cpp
// Time: O(2^n) * k, Space: O(n!/(n!-k!)k! * k)
class Solution {
public:
    vector<vector<int>> combine(int n, int k) {
        vector<vector<int>> result;
        int upperBound = 1 << n;
        for (int mask = 0; mask < upperBound; mask++) {
            if (k == getCountOnes(mask)) {
                vector<int> combo = getCombo(mask, n);
                result.push_back(combo);
            }
        }

        return result;
    }

private:
    int getCountOnes(int bin) {
        return __builtin_popcount(bin);
    }

    vector<int> getCombo(int bin, int n) {
        vector<int> combo;

        for (int shift = 0; shift < n; shift++) {
            if (bin & (1 << shift)) {
                combo.push_back(shift + 1);
            }
        }

        return combo;
    }
};
```

## 806. Number of Lines To Write String
```cpp
class Solution {
public:
    vector<int> numberOfLines(vector<int>& widths, string s) {
        int totalLines = 1;
        int currLineWidth = 0;

        for (int i = 0; i < s.size(); i++) {
            int charWidth = widths[s[i] - 'a'];

            if (currLineWidth + charWidth > 100) {
                totalLines++;
                currLineWidth = charWidth;
                continue;
            }

            currLineWidth += charWidth;
        }

        return {totalLines, currLineWidth};
    }
};
```

## 575. Distribute Candies
```cpp
// Time: O(n), Space: O(n)
class Solution {
public:
    int distributeCandies(vector<int>& candyType) {
        unordered_set<int> unique(candyType.begin(), candyType.end());
        return min(unique.size(), candyType.size() / 2);
    }
};
```

## 1318. Minimum Flips to Make a OR b Equal to c
```cpp
// Time: O(1), Space: O(1)
class Solution {
public:
    int minFlips(int a, int b, int c) {
        int count = 0;
        for (int i = 0; i < 32; i++) {
            int mask = 1 << i;
            // if bit set
            if (c & mask) {
                // if both bit in a and b is 0 then need to flip one
                count += ((a & mask) == 0) && ((b & mask) == 0);
            }
            // if bit not set
            else {
                // flip a and b to zero if either is one
                count += ((a & mask) != 0);
                count += ((b & mask) != 0);
            }
        }

        return count;
    }
};
```

## 884. Uncommon Words from Two Sentences
```cpp
// Time: O(s1 + s2), Space: O(s1 + s2);
class Solution {
public:
    vector<string> uncommonFromSentences(string s1, string s2) {
        unordered_map<string, int> sMap;

        vector<string> s1Words { getWords(s1) };
        for (const string& word: s1Words) {
            sMap[word]++;
        }

        vector<string> s2Words { getWords(s2) };
        for (const string& word: s2Words) {
            sMap[word]++;
        }

        vector<string> result;

        for (const auto [key, value] : sMap) {
            if (value == 1) {
                result.push_back(key);
            }
        }

        return result;
    }

private:
    vector<string> getWords(const string& s) {
        vector<string> result;
        
        string word;
        for (const char c : s) {
            if (c == ' ') {
                result.push_back(word);
                word = "";
            } else {
                word.push_back(c);
            }
        }

        result.push_back(word);

        return result;
    }
};
```

## 1822. Sign of the Product of an Array
```cpp
// Time: O(n), Space: O(1)
class Solution {
public:
    int arraySign(vector<int>& nums) {
        int product = 1;
        for (const int num : nums) {
            if (num == 0) {
                return 0;
            }

            if (num > 0) {
                product *= 1;
            } else {
                product *= -1;
            }
        }

        return product;
    }
};
```

## 973. K Closest Points to Origin
### Priority Queue
```cpp
// Time: O(n * log k), Space: O(k)
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        priority_queue<vector<int>, vector<vector<int>>, Compare> pq;

        for (const auto& point : points) {
            pq.push(point);

            if (pq.size() > k) {
                pq.pop();
            }
        }

        vector<vector<int>> result;
        while (pq.size()) {
            result.push_back(pq.top());
            pq.pop();
        }

        return result;
    }

private:
    struct Compare {
        bool operator()(const vector<int>& lhs, const vector<int>& rhs) {
            auto dist1 = pow(lhs[0], 2) + pow(lhs[1], 2);
            auto dist2 = pow(rhs[0], 2) + pow(rhs[1], 2);
            return dist1 < dist2;
        }
    };
};
```

### Quick Select
```cpp
// Time: O(n), Space: O(1)
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        quickSelect(points, k);
        return vector(points.begin(), points.begin() + k);
    }

private:
    void quickSelect(vector<vector<int>>& points, int k) {
        int left = 0;
        int right = points.size() - 1;

        while (left < right) {
            // get random index
            random_device rd;
            mt19937 gen(rd());
            uniform_int_distribution<> distrib(left, right);

            // swap with right
            swap(points[distrib(gen)], points[right]);

            // lomuto partition
            int partitionIndex = partition(points, left, right);
            if (partitionIndex == k) {
                return;
            }

            if (partitionIndex < k) {
                left = partitionIndex + 1;
            } else {
                right = partitionIndex - 1;
            }
        }
    }

    int partition(vector<vector<int>>& points, int left, int right) {
        int partitionIndex = left - 1;
        int partitionDist = pow(points[right][0], 2) + pow(points[right][1], 2);

        for (int i = left; i < right; i++) {
            int currDist = pow(points[i][0], 2) + pow(points[i][1], 2);
            if (currDist <= partitionDist) {
                partitionIndex++;
                swap(points[i], points[partitionIndex]);
            }
        }

        partitionIndex++;
        swap(points[right], points[partitionIndex]);
        return partitionIndex;
    }
};
```

### nth_element
```cpp
// Time: O(n), Space: O(1)
class Solution {
public:
    vector<vector<int>> kClosest(vector<vector<int>>& points, int k) {
        nth_element(points.begin(), points.begin() + k - 1, points.end(), [](const auto& point1, const auto& point2) {
            return pow(point1[0], 2) + pow(point1[1], 2) < pow(point2[0], 2) + pow(point2[1], 2);
        });

        return vector<vector<int>>(points.begin(), points.begin() + k);
    }
};
```

## 215. Kth Largest Element in an Array
### nth_element
```cpp
// Time: O(n), Space: O(1)

class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        nth_element(nums.begin(), nums.begin() + k - 1, nums.end(), greater<int>());
        return nums[k - 1];
    }
};
```

### Min Heap
```cpp
// Time: O(n log k), Space: O(k)
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        priority_queue<int, vector<int>, greater<int>> pq;
        
        for (const int num : nums) {
            pq.push(num);

            if (pq.size() > k) {
                pq.pop();
            }
        }

        return pq.top();
    }
};
```

### Quick Select
```cpp
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        quickSelect(nums.begin(), nums.begin() + k - 1, nums.end(), greater<int>());
        return nums[k - 1];
    }

private:
    template <class RandIt, class Compare>
    void quickSelect(RandIt left, RandIt k, RandIt right, Compare cmp) {
        right = right - 1;

        while (left < right) {
            // random index
            static random_device rd;
            static mt19937 gen(rd());
            uniform_int_distribution<> dist(0, distance(left, right) - 1);

            // swap with right
            const auto randomIndex = left + dist(gen);
            iter_swap(randomIndex, right - 1);

            // lomuto partition
            RandIt partitionIndex { partition(left, right, cmp) };

            if (partitionIndex == k) {
                return;
            }

            if (partitionIndex < k) {
                left = partitionIndex + 1;
            } else {
                right = partitionIndex - 1;
            }
        }
    }

    template <class RandIt, class Compare>
    RandIt partition(RandIt left, RandIt right, Compare cmp) {
        RandIt partitionIndex = left - 1;
        for (auto it { left }; it < right; it++) {
            if (cmp(*it, *right)) {
                partitionIndex++;
                iter_swap(partitionIndex, it);
            }
        }

        partitionIndex++;
        iter_swap(partitionIndex, right);
        return partitionIndex;
    }
};
```

## 2043. Simple Bank System
```cpp
class Bank {
public:
    Bank(vector<long long>& balance) {
        swap(balance, _accounts);
    }
    
    bool transfer(int account1, int account2, long long money) {
        if (!validAccount(account2)) {
            return false;
        }
        
        return withdraw(account1, money) && deposit(account2, money);
    }
    
    bool deposit(int account, long long money) {
        if (!validAccount(account)) {
            return false;
        }

        _accounts[account - 1] += money;
        return true;        
    }
    
    bool withdraw(int account, long long money) {
        if (!validAccount(account) || !sufficientFunds(account, money)) {
            return false;
        }

        _accounts[account - 1] -= money;
        return true;
    }

private:
    vector<long long> _accounts;

    inline bool validAccount(int account) {
        return 0 <= account - 1 && account - 1 < _accounts.size();
    }

    inline bool sufficientFunds(int account, long long money) {
        return _accounts[account - 1] >= money;
    }
};

/**
 * Your Bank object will be instantiated and called as such:
 * Bank* obj = new Bank(balance);
 * bool param_1 = obj->transfer(account1,account2,money);
 * bool param_2 = obj->deposit(account,money);
 * bool param_3 = obj->withdraw(account,money);
 */
```

## 1249. Minimum Remove to Make Valid Parentheses
```cpp
class Solution {
public:
    string minRemoveToMakeValid(string s) {
        string result;

        vector<int> include(s.size(), false);
        vector<int> stack;
        for (int i = 0; i < s.size(); i++) {
            if (s[i] == '(') {
                stack.push_back(i);
            }
            else if (s[i] == ')') {
                if (!stack.empty()) {
                    int index = stack.back();
                    stack.pop_back();

                    include[index] = true;
                    include[i] = true;
                }
            }
            else {
                include[i] = true;
            }
        }

        for (int i = 0; i < include.size(); i++) {
            if (include[i]) {
                result.push_back(s[i]);
            }
        }

        return result;
    }
};
```

## 1833. Maximum Ice Cream Bars
```cpp
class Solution {
public:
    int maxIceCream(vector<int>& costs, int coins) {
        sort(costs.begin(), costs.end());

        int count = 0;
        for (int i = 0; i < costs.size(); i++) {
            if (coins - costs[i] < 0) {
                break;
            }
            count++;
            coins -= costs[i];
        }

        return count;
    }
};
```

## 1582. Special Positions in a Binary Matrix
```cpp
class Solution {
public:
    int numSpecial(vector<vector<int>>& mat) {
        vector<int> rowCounts(mat.size(), 0);
        vector<int> colCounts(mat[0].size(), 0);

        for (int row = 0; row < mat.size(); row++) {
            for (int col = 0; col < mat[0].size(); col++) {
                rowCounts[row] += mat[row][col];
                colCounts[col] += mat[row][col];
            }
        }

        int count = 0;
        for (int row = 0; row < mat.size(); row++) {
            for (int col = 0; col < mat[0].size(); col++) {
                count += mat[row][col] && rowCounts[row] == 1 && colCounts[col] == 1;
            }
        }

        return count;
    }
};
```

## 1780. Check if Number is a Sum of Powers of Three
```cpp
class Solution {
public:
    bool checkPowersOfThree(int n) {
        for (int power = 0; pow(3, power) <= n; power++) {
            powersOf3.push_back(pow(3, power));
        }
        

        int curr = n;
        for (int i = powersOf3.size() - 1; i >= 0 && curr > 0; i--) {
            if (curr - powersOf3[i] >= 0) {
                curr -= powersOf3[i];
            }
        }

        return curr == 0;
    }

private:
    vector<int> powersOf3;
};
```

## 1414. Find the Minimum Number of Fibonacci Numbers Whose Sum Is K
```cpp
class Solution {
public:
    int findMinFibonacciNumbers(int k) {
        vector<int> fibs {1, 1};
        
        // pre-comute 
        while (true) {
            const int next = fibs[fibs.size() - 1] + fibs[fibs.size() - 2];
            if (next > k) {
                break;
            }

            fibs.push_back(next);
        }
        
        // Zeckendorf's theorem
        int curr = k;
        int count = 0;
        for (int i = fibs.size() - 1; i >= 0 && curr >= 0; i--) {
            while (curr - fibs[i] >= 0) {
                curr -= fibs[i];
                count++;
            }
        }

        return count;
    }
};
```

## 2138. Divide a String Into Groups of Size k
```cpp
class Solution {
public:
    vector<string> divideString(string s, int k, char fill) {
        vector<string> result;
        
        for (int start = 0; start < s.size(); start += k) {
            string group = s.substr(start, k);
            if (group.size() < k) {
                group += string(k - group.size(), fill);
            }

            result.push_back(group);
        }

        return result;   
    }
};
```

## 872. Leaf-Similar Trees
### DFS
```cpp
// Time: O(n + m), Space: O(n + m)
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
    bool leafSimilar(TreeNode* root1, TreeNode* root2) {
        vector<int> leaves1 = getLeaves(root1);
        vector<int> leaves2 = getLeaves(root2);
        return leaves1 == leaves2;
    }

private:
    vector<int> getLeaves(TreeNode* root) {
        vector<int> leaves;
        traverse(root, leaves);
        return leaves;
    }

    void traverse(TreeNode* root, vector<int>& leaves) {
        if (!root) return;

        if (!root->left && !root->right) {
            leaves.push_back(root->val);
            return;
        }

        traverse(root->left, leaves);
        traverse(root->right, leaves);
    }
};
```

## 997. Find the Town Judge
```cpp
class Solution {
public:
    int findJudge(int n, vector<vector<int>>& trust) {
        vector<int> indegree(n + 1);
        vector<int> outdegree(n + 1);

        for (auto& edge : trust) {
            indegree[edge[1]] += 1;
            outdegree[edge[0]] += 1;
        }

        for (int i = 1; i <= n; i++) {
            if (outdegree[i] == 0 && indegree[i] == n - 1) {
                return i;
            }
        }

        return -1;
    }
};
```

## 901. Online Stock Span
```cpp
class StockSpanner {
public:
    StockSpanner() {
    }
    
    int next(int price) {
        while (stack.size() && stack.back().second <= price) {
            stack.pop_back();
        }

        day++;
        int span = day;
        if (stack.size()) {
            span = day - stack.back().first;
        }

        stack.push_back({day, price});
        return span;
    }

private:
    vector<pair<int, int>> stack;
    int day = 0;
};

/**
 * Your StockSpanner object will be instantiated and called as such:
 * StockSpanner* obj = new StockSpanner();
 * int param_1 = obj->next(price);
 */
```

## 413. Arithmetic Slices
```cpp
class Solution {
public:
    int numberOfArithmeticSlices(vector<int>& nums) {
        int result = 0;
        int count = 0;
        for (int i = 2; i < nums.size(); i++) {
            if (nums[i - 1] - nums[i] == nums[i - 2] - nums[i - 1]) {
                count++;
                result += count;
            } else {
                count = 0;
            }
        }

        return result;
    }
};
```

## 191. Number of 1 Bits
### Popcount
```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        return __builtin_popcount(n);
    }
};
```

### Bit Manipulation
```cpp
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int count = 0;
        while (n) {
            n &= (n - 1);
            count++;
        }
        return count;
    }
};
```

## 1227. Airplane Seat Assignment Probability
```cpp
class Solution {
public:
    double nthPersonGetsNthSeat(int n) {
        if (n == 1) {
            return 1;
        }
        
        return 0.5; 
    }
};
```

## 144. Binary Tree Preorder Traversal
### Recursive
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
    vector<int> preorderTraversal(TreeNode* root) {
        vector<int> result;
        dfs(root, result);
        return result;
    }

private:
    void dfs(TreeNode* root, vector<int>& result) {
        if (!root) {
            return;
        }

        result.push_back(root->val);
        dfs(root->left, result);
        dfs(root->right, result);
    }
};
```

## 1192. Critical Connections in a Network
```cpp
/*
Time: V+E
Space: V
*/
class Solution {
private:
    void findBridges(int vertex, vector<vector<int>>& result, vector<vector<int>>& graph, vector<int>& parent, vector<int>& id, vector<int>& lowlink, int& time)
    {
        id[vertex] = time;
        lowlink[vertex] = time;
        ++time;

        for (const auto neighbor : graph[vertex])
        {
            if (parent[vertex] == neighbor) continue;

            // if neighbor hasn't been visited
            if (id[neighbor] == -1)
            {
                parent[neighbor] = vertex;
                findBridges(neighbor, result, graph, parent, id, lowlink, time);
                lowlink[vertex] = std::min(lowlink[vertex], lowlink[neighbor]);
                
                // check if bridge
                if (id[vertex] < lowlink[neighbor])
                {
                    result.push_back({vertex, neighbor});    
                }
            }
            else
            {
                lowlink[vertex] = std::min(lowlink[vertex], id[neighbor]);
            }
        }
    }

public:
    vector<vector<int>> criticalConnections(int n, vector<vector<int>>& connections) {
        // Create graph
        vector<vector<int>> graph;
        graph.resize(n);

        for (const auto connection : connections)
        {
            graph[connection[0]].push_back(connection[1]);
            graph[connection[1]].push_back(connection[0]);
        }

        // Tarjan's SCC
        vector<vector<int>> result;
        vector<int> parent(n, -1);
        vector<int> id(n, -1);
        vector<int> lowlink(n, -1);
        int time = 0;
        
        findBridges(0, result, graph, parent, id, lowlink, time);

        return result;    
    }
};
```

## 1568. Minimum Number of Days to Disconnect Island
```cpp
class Solution {
private:
    void tarjan(vector<vector<int>>& graph, int vertex, vector<int>& lowlink, vector<int>& id, vector<int>& parent, int& time, vector<bool>& points)
    {
        id[vertex] = time;
        lowlink[vertex] = time;
        ++time;

        int childCount = 0;

        for (const auto neighbor : graph[vertex])
        {
            if (parent[vertex] == neighbor) continue;

            if (id[neighbor] == -1)
            {
                parent[neighbor] = vertex;
                ++childCount;
                tarjan(graph, neighbor, lowlink, id, parent, time, points);
                lowlink[vertex] = min(lowlink[vertex], lowlink[neighbor]);

                // child count if root
                if (parent[vertex] == -1 && childCount > 1)
                {
                    points[vertex] = true;
                } 
                
                if (parent[vertex] != -1 && id[vertex] <= lowlink[neighbor])
                {
                    points[vertex] = true;
                }
            }
            else
            {
                lowlink[vertex] = min(lowlink[vertex], id[neighbor]);
            }
        }
    }

    int findArticulationPoints(vector<vector<int>>& graph)
    {
        int size = graph.size();
        vector<int> lowlinks(size, -1);
        vector<int> ids(size, -1);
        vector<int> parents(size, -1);
        vector<bool> points(size, false);
        int time = 0;

        int components = 0;
        for (auto vertex = 0; vertex < graph.size(); ++vertex)
        {
            if (ids[vertex] == -1)
            {
                ++components;
                tarjan(graph, vertex, lowlinks, ids, parents, time, points);
            }
                
        }

        int numOfArticulationPoints = 0;
        for (const auto p : points)
        {
            numOfArticulationPoints += p;
        }

        if (components != 1) return 0;
        if (numOfArticulationPoints > 0) return 1;
        return 2;
    }

    vector<vector<int>> buildGraph(vector<vector<int>>& grid)
    {
        const auto m = grid.size();
        const auto n = grid[0].size();

        int numOfOnes = 0;
        for (auto row = 0; row < m; ++row)
        {
            for (auto col = 0; col < n; ++col)
            {
                numOfOnes += grid[row][col];
            }
        }

        vector<vector<int>> graph;
        graph.resize(numOfOnes);

        unordered_map<int, int> vertexIds;
        int id = 0;

        const array<pair<int, int>, 4> dirs {{{1, 0}, {0, 1}}};
        for (auto row = 0; row < m; ++row)
        {
            for (auto col = 0; col < n; ++col)
            {
                if (grid[row][col] == 0) continue;

                for (const auto [deltaRow, deltaCol] : dirs) {
                    const auto nextRow = row + deltaRow;
                    const auto nextCol = col + deltaCol;

                    if (nextRow < 0 || nextRow >= m || nextCol < 0 || 
                        nextCol >= n || grid[nextRow][nextCol] == 0) continue;

                    int v = row * n + col;
                    int u = nextRow * n + nextCol;

                    if (!vertexIds.count(v))
                    {
                        vertexIds[v] = id;
                        ++id;
                    }

                    if (!vertexIds.count(u))
                    {
                        vertexIds[u] = id;
                        ++id;
                    }

                    graph[vertexIds[v]].push_back(vertexIds[u]);
                    graph[vertexIds[u]].push_back(vertexIds[v]);
                }
            }
        }

        return graph;
    }
    
public:
    int minDays(vector<vector<int>>& grid) {
        if (grid.empty() || grid[0].empty()) return 0;

        auto graph = buildGraph(grid);
        if (graph.size() <= 1) return graph.size();

        return findArticulationPoints(graph);
    }
};
```

## 1489. Find Critical and Pseudo-Critical Edges in Minimum Spanning Tree
```cpp
/*
Time: O(E^2)
Space: O(E)
*/

class UnionFind
{
private:
    vector<int> m_parents;
    vector<int> m_sizes;
    int m_numOfComponents;

    int find(int a)
    {
        auto root = a;
        while (root != m_parents[root])
        {
            root = m_parents[root];
        }
        
        while (a != root)
        {
            auto next = m_parents[a];
            m_parents[a] = root;
            a = next;
        }

        return root;
    }

public:
    UnionFind(int n) : m_parents(n), m_sizes(n), m_numOfComponents(n) {
        iota(m_parents.begin(), m_parents.end(), 0);
        fill(m_sizes.begin(), m_sizes.end(), 1);
    }

    bool join(int a, int b)
    {
        const auto parentA = find(a);
        const auto parentB = find(b);

        if (parentA == parentB) return false;

        if (m_sizes[parentA] < m_sizes[parentB])
        {
            m_sizes[parentB] += m_sizes[parentA];
            m_parents[parentA] = parentB;
        }
        else
        {
            m_sizes[parentA] += m_sizes[parentB];
            m_parents[parentB] = parentA;
        }

        --m_numOfComponents;
        return true;
    }

    int numOfComponents()
    {
        return m_numOfComponents;
    }
};

class Solution {
private:
    int kruskals(int n, int skipIndex, int includeIndex, vector<vector<int>>& edges, vector<int>& sortedEdgeIndices)
    {
        int mstValue = 0;
        UnionFind unionFind(n);

        if (includeIndex != -1)
        {
            const auto edge = edges[includeIndex];
            unionFind.join(edge[0], edge[1]);
            mstValue += edge[2];
        }

        for (const auto index : sortedEdgeIndices)
        {
            if (index == skipIndex) continue;

            const auto edge = edges[index];
            if (!unionFind.join(edge[0], edge[1]))
            {
                continue;
            }

            mstValue += edge[2];
        }
        
        if (unionFind.numOfComponents() == 1)
        {
            return mstValue;
        }

        // if mst doesn't connect all nodes than invalid
        return std::numeric_limits<int>::max();
    }

    vector<int> sortEdgesByWeight(vector<vector<int>>& edges)
    {
        vector<vector<int>> weightBuckets;
        weightBuckets.resize(1001);

        for (auto i = 0; i < edges.size(); ++i)
        {
            const auto weight = edges[i][2];
            weightBuckets[weight].push_back(i);
        }

        vector<int> sortedEdgeIndices;
        for (auto weight = 0; weight < weightBuckets.size(); ++weight)
        {
            for (auto index = 0; index < weightBuckets[weight].size(); ++index)
            {
                sortedEdgeIndices.push_back(weightBuckets[weight][index]);
            }
        }

        return sortedEdgeIndices;
    }

public:
    vector<vector<int>> findCriticalAndPseudoCriticalEdges(int n, vector<vector<int>>& edges) {
        vector<int> criticalEdgeIndices;
        vector<int> pseudoCriticalEdgeIndices;

        vector<int> sortedEdgeIndices = sortEdgesByWeight(edges);

        const auto mstValue = kruskals(n, -1, -1, edges, sortedEdgeIndices);
        for (auto index = 0; index < edges.size(); index++)
        {
            // exclude
            auto currMstValue_exclude = kruskals(n, index, -1, edges, sortedEdgeIndices);
            if (currMstValue_exclude > mstValue)
            {
                criticalEdgeIndices.push_back(index);
                continue;
            }

            // include
            const auto currMstValue_include = kruskals(n, -1, index, edges, sortedEdgeIndices);
            if (currMstValue_include == mstValue)
            {
                pseudoCriticalEdgeIndices.push_back(index);
            }
        }
        
        return {criticalEdgeIndices, pseudoCriticalEdgeIndices};
    }
};
```

## 2097. Valid Arrangement of Pairs
```cpp
// Time: O(E)
// Space: O(E)
class Solution {
public:
    vector<vector<int>> validArrangement(vector<vector<int>>& pairs) {
        // build graph
        unordered_map<int, vector<int>> graph;

        // in and out degrees
        unordered_map<int, int> degrees;
        for (const auto& pair : pairs)
        {
            const auto from = pair[0];
            const auto to = pair[1];
            graph[from].push_back(to);

            // outdegree
            ++degrees[from];
            // indegree
            --degrees[to];
        }

        // if all nodes have even in and out then start anywhere
        // if degree == 1 (1 more out than in) that is start
        int startNode = -1;
        for (const auto [node, degree] : degrees)
        {
            if (startNode == -1 || degree == 1)
            {
                startNode = node;
            }
        }
        
        if (startNode == -1) return {};

        vector<int> path;
        // hierholzer's algo - find euler path
        hierholzer(path, graph, startNode);

        // construct pairs from euler path
        vector<vector<int>> result;
        for (auto i = path.size() - 1; i > 0; --i)
        {
            int curr = path[i];
            int next = path[i - 1];

            result.push_back({curr, next});
        }

        return result;
    }

private:
    void hierholzer(vector<int>& path, unordered_map<int, vector<int>>& graph, int node)
    {
        vector<int> stack = { node };
        while (stack.size())
        {
            auto curr = stack.back();
            while (graph[curr].size())
            {
                const auto child = graph[curr].back();
                graph[curr].pop_back();
                curr = child;
                stack.push_back(curr);
            }

            stack.pop_back();
            path.push_back(curr);
        }
    }
};
```

## 332. Reconstruct Itinerary
```cpp
class Solution {
    using Graph = std::unordered_map<std::string, std::priority_queue<std::string, vector<std::string>, std::greater<std::string>>>;

private:
    void hierholzer(std::vector<std::string>& result, Graph& adjList, const std::string& node)
    {
        // loop through children
        while (adjList[node].size())
        {
            const auto child = adjList[node].top();
            adjList[node].pop();

            hierholzer(result, adjList, child);
        }

        result.push_back(node);
    }

public:
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        Graph adjList;

        for (const auto ticket : tickets)
        {
            const auto from = ticket[0];
            const auto to = ticket[1];
            adjList[from].push(to);
        }

        std::vector<std::string> stack;
        std::vector<std::string> result;
        hierholzer(result, adjList, "JFK");
        std::reverse(result.begin(), result.end());
        return result;
    }
};
```

## 382. Linked List Random Node
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
private:
    ListNode* _head;
    random_device _rd;
    mt19937 _gen;
public:
    Solution(ListNode* head) : _head(head) {
        _gen = mt19937{ _rd() };
    }
    
    int getRandom() {
        auto randomVal = -1;
        auto k = 0;
        auto curr = _head;
        while (curr)
        {
            uniform_int_distribution dist(0, k);
            if (dist(_gen) == 0)
            {
                randomVal = curr->val;
            }

            curr = curr->next;
            ++k;
        }

        return randomVal;
    }
};

/**
 * Your Solution object will be instantiated and called as such:
 * Solution* obj = new Solution(head);
 * int param_1 = obj->getRandom();
 */
```

## 42. Trapping Rain Water
### DP
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        const auto n = height.size();

        std::vector<int> rightMax;
        rightMax.reserve(n);

        // calc right
        auto currRightMax = 0;
        for (int i = n - 1; i >= 0; --i)
        {
            rightMax[i] = currRightMax;
            currRightMax = std::max(currRightMax, height[i]);
        }

        auto currLeftMax = 0;
        auto trappedWater = 0;
        for (int i = 0; i < n; ++i)
        {
            const auto boundHeight = std::min(currLeftMax, rightMax[i]);
            const auto water = boundHeight - height[i];

            if (water > 0)
            {
                trappedWater += water;
            }

            currLeftMax = std::max(currLeftMax, height[i]);
        }

        return trappedWater;
    }
};
```

### Two Pointer
```cpp
class Solution {
public:
    int trap(vector<int>& height) {
        int trappedWater = 0;

        int leftMax = 0;
        int rightMax = 0;

        int left = 0;
        int right = height.size() - 1;

        while (left < right)
        {
            leftMax = std::max(leftMax, height[left]);
            rightMax = std::max(rightMax, height[right]);

            if (leftMax <= rightMax)
            {
                trappedWater += leftMax - height[left];
                ++left;
            }
            else
            {
                trappedWater += rightMax - height[right];
                --right;
            }
        }

        return trappedWater;
    }
};
```

## 2275. Largest Combination With Bitwise AND Greater Than Zero
```cpp
class Solution {
public:
    int largestCombination(vector<int>& candidates) {
      int max = 0;
      for (auto position = 0; position < 32; ++position)
      {
        auto positionMask = 1 << position;
        auto numsWithOneAtPosition = 0;
        for (const auto candidate : candidates)
        {
          if (candidate & positionMask)
          {
            ++numsWithOneAtPosition;
          }
        }

        max = std::max(max, numsWithOneAtPosition);
      }
      return max;
    }
};
```

## 226. Invert Binary Tree
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
    TreeNode* invertTree(TreeNode* root) {
        if (!root) return nullptr;

        auto temp = root->left;
        root->left = root->right;
        root->right = temp;

        invertTree(root->left);
        invertTree(root->right);

        return root;
    }
};
```

## 704. Binary Search
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;

        while (left <= right) {
            int mid = (right - left) / 2 + left;

            if (nums[mid] == target) {
                return mid;
            }
            else if (nums[mid] < target) {
                left = mid + 1;
            }
            else {
                right = mid - 1;
            }
        }

        return -1;
    }
};
```

## 70. Climbing Stairs
```cpp
class Solution {
public:
    int climbStairs(int n) {
        if (n <= 0) return 0;
        if (n == 1) return 1;

        auto dp2 = 1;
        auto dp1 = 2;       
        for (auto step = 3; step <= n; ++step) {
            auto dp = dp1 + dp2;
            dp2 = dp1;
            dp1 = dp;
        }
        
        return dp1;
    }
};
```

## 104. Maximum Depth of Binary Tree
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
    int maxDepth(TreeNode* root) {
        if (!root) return 0;
        
        const auto left = maxDepth(root->left);
        const auto right = maxDepth(root->right);

        return 1 + std::max(left, right);
    }
};
```

## 141. Linked List Cycle
```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode(int x) : val(x), next(NULL) {}
 * };
 */
class Solution {
public:
    bool hasCycle(ListNode *head) {
        if (!head || !head->next) return false;

        auto fast = head->next;
        auto slow = head;

        while (fast && fast->next)
        {
            fast = fast->next->next;
            slow = slow->next;

            if (fast == slow) return true;
        }
        
        return false;
    }
};
```

## 543. Diameter of Binary Tree
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
private:
    int _diameterOfBinaryTree(TreeNode* root, int& diameter) {
        if (!root) return 0;

        const auto left = _diameterOfBinaryTree(root->left, diameter);
        const auto right = _diameterOfBinaryTree(root->right, diameter);

        diameter = std::max(diameter, left + right);
        return 1 + std::max(left, right);
    }

public:
    int diameterOfBinaryTree(TreeNode* root) {
        int diameter = 0;
        _diameterOfBinaryTree(root, diameter);
        return diameter;
    }
};
```

## 409. Longest Palindrome
```cpp
class Solution {
public:
    int longestPalindrome(string s) {
        int length = 0;
        unordered_map<char, int> counts;
        for (const auto c : s) {
            ++counts[c];
        }

        int oddSeen = false;
        for (const auto [key, value] : counts)
        {
            if (value & 1) {
                if (!oddSeen) {
                    oddSeen = true;
                    ++length;
                }

                length += value - 1;
            }
            else
            {
                length += value;
            }
        }

        return length;
    }
};
```

## 235. Lowest Common Ancestor of a Binary Search Tree
```cpp
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */

class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if (!root) return nullptr;
        if (root == p || root == q) return root;

        if (p->val < root->val && q->val < root->val) {
            return lowestCommonAncestor(root->left, p, q);
        } 
        
        if (root->val < p->val && root->val < q->val) {
            return lowestCommonAncestor(root->right, p, q);
        }
        
        return root;
    }
};
```

## 110. Balanced Binary Tree
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
private: 
    int _isBalanced(TreeNode* root) {
        if (!root) return 0;

        const auto left = _isBalanced(root->left);
        if (left == -1) return -1;

        const auto right = _isBalanced(root->right);
        if (right == -1) return -1;

        if (std::abs(left - right) > 1) {
            return -1;
        }

        return std::max(left, right) + 1;
    }

public:
    bool isBalanced(TreeNode* root) {
        return _isBalanced(root) != -1;
    }
};
```

## 67. Add Binary
```cpp
class Solution {
public:
    string addBinary(string a, string b) {
        std::string output;

        int aIndex = a.size() - 1;
        int bIndex = b.size() - 1;
        auto carry = 0;

        while (carry || aIndex >= 0 || bIndex >= 0) {
            auto aBit = 0;
            if (aIndex >= 0) {
                aBit = a[aIndex] - '0';
            }

            auto bBit = 0;
            if (bIndex >= 0) {
                bBit = b[bIndex] - '0';
            }

            const auto sum = aBit + bBit + carry;
            char bit = (sum % 2) + '0';
            output.push_back(bit);
            carry = sum / 2;

            --aIndex;
            --bIndex;     
        }
        
        std::reverse(std::begin(output), std::end(output));
        return output;
    }
};
```

## 100. Same Tree
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
    bool isSameTree(TreeNode* p, TreeNode* q) {
        if (!p and !q) return true;
        if (!p || !q) return false;
        
        return p->val == q->val &&
               isSameTree(p->left, q->left) && 
               isSameTree(p->right, q->right);
    }
};
```

## 3. Longest Substring Without Repeating Characters
```cpp
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        unordered_set<char> seen;

        int longest = 0;
        int left = 0;
        for (int right = 0; right < s.size(); ++right) {
            while (seen.count(s[right]) != 0) {
                seen.erase(s[left]);
                ++left;
            }

            seen.insert(s[right]);

            longest = std::max(right - left + 1, longest);
        }

        return longest;
    }
};
```

## 102. Binary Tree Level Order Traversal
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
    vector<vector<int>> levelOrder(TreeNode* root) {
        if (!root) return {};

        vector<vector<int>> result;

        std::deque<TreeNode*> queue;
        queue.push_back(root);
        
        while (queue.size()) {
            const auto size = queue.size();
            vector<int> level;
            for (int i = 0; i < size; ++i) {
                const auto node = queue.front();
                queue.pop_front();

                level.push_back(node->val);

                if (node->left) {
                    queue.push_back(node->left);
                }

                if (node->right) {
                    queue.push_back(node->right);   
                }
            }
            result.push_back(level);
        }

        return result;
    }
};
```

## 238. Product of Array Except Self
```cpp
class Solution {
public:
    vector<int> productExceptSelf(vector<int>& nums) {
        vector<int> result(nums.size());
        vector<int> rightProducts(std::begin(nums), std::end(nums));

        for (int i = rightProducts.size() - 2; i >= 0; --i) {
            rightProducts[i] *= rightProducts[i + 1];
        }

        int leftProduct = 1;
        for (int i = 0; i < nums.size(); ++i) {
            const auto right = i + 1 >= nums.size() ? 1 : rightProducts[i + 1];
            const auto product = leftProduct * right;
            leftProduct *= nums[i];
            result[i] = product;
        }

        return result;
    }
};
```

## 2574. Left and Right Sum Differences
```cpp
class Solution {
public:
    vector<int> leftRightDifference(vector<int>& nums) {
        vector<int> result(nums.size());

        int currLeftSum = 0;
        int currRightSum = std::accumulate(nums.begin(), nums.end(), 0);
        for (int i = 0; i < nums.size(); ++i) {
            currRightSum -= nums[i];
            result[i] = std::abs(currLeftSum - currRightSum);
            currLeftSum += nums[i];
        }

        return result;
    }
};
```

## 724. Find Pivot Index
```cpp
class Solution {
public:
    int pivotIndex(vector<int>& nums) {
        int rightSum = std::accumulate(nums.begin(), nums.end(), 0);
        int leftSum = 0;
        for (auto i = 0; i < nums.size(); ++i) {
            rightSum -= nums[i];

            if (leftSum == rightSum) {
                return i;
            }
            
            leftSum += nums[i];
        }

        return -1;
    }
};
```

## 2955. Number of Same-End Substrings
```
class Solution {
public:
    vector<int> sameEndSubstringCount(string s, vector<vector<int>>& queries) {
        vector<int> result;

        // countOfCharsPerIndex[i] = count of chars we've seen from 0...i
        vector<vector<int>> countOfCharsPerIndex(s.size() + 1, vector<int>(26));

        for (auto strIndex = 1; strIndex <= s.size(); ++strIndex) {
            // Copy previous counts
            countOfCharsPerIndex[strIndex] = countOfCharsPerIndex[strIndex - 1];
            const auto charIndex = s[strIndex - 1] - 'a';
            ++countOfCharsPerIndex[strIndex][charIndex];
        }

        for (const auto query : queries) {
            const auto left = query[0];
            const auto right = query[1];

            int substrings = 0;
            for (auto c = 0; c < 26; ++c) {
                const auto count = countOfCharsPerIndex[right + 1][c] - countOfCharsPerIndex[left][c];
                substrings += count * (count + 1) / 2;
            }

            result.push_back(substrings);
        }
        return result;
    }
};
```

## 1371. Find the Longest Substring Containing Vowels in Even Counts
```cpp
class Solution {
public:
    int findTheLongestSubstring(string s) {
        int maxLength = 0;

        std::unordered_map<int, int> bitMaskMap = {{0, -1}};
        int bitMask = 0;

        const std::unordered_set<char> vowels = {'a', 'e', 'i', 'o', 'u'};
        for (auto right = 0; right < s.size(); ++right) {
            if (vowels.count(s[right]) != 0) {
                const auto bitPosition = s[right] - 'a';
                const auto shiftedBit = 1 << bitPosition;
                bitMask ^= shiftedBit;
            }

            if (bitMaskMap.count(bitMask) == 0) {
                bitMaskMap[bitMask] = right;
                continue;
            }

            const auto left = bitMaskMap[bitMask];
            const auto distance = right - left;
            maxLength = std::max(distance, maxLength);
        }

        return maxLength;
    }
};
```

## 2559. Count Vowel Strings in Ranges
```cpp
class Solution {
private:
    const std::unordered_set<char> vowels = {'a', 'e', 'i', 'o', 'u'};
    bool isVowelString(std::string str) {
        return vowels.count(str.front()) != 0 && vowels.count(str.back()) != 0;
    }

public:
    vector<int> vowelStrings(vector<string>& words, vector<vector<int>>& queries) {
        std::vector<int> prefixSums(words.size() + 1, 0);
        auto writeIndex = 1;
        for (const auto word : words) {
            prefixSums[writeIndex] = prefixSums[writeIndex - 1] + isVowelString(word);
            ++writeIndex;
        }

        std::vector<int> result(queries.size(), 0);
        writeIndex = 0;
        for (const auto query : queries) {
            const auto left = query[0];
            const auto right = query[1];

            result[writeIndex] = prefixSums[right + 1] - prefixSums[left];
            ++writeIndex;
        }

        return result;
    }
};
```