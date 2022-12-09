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
public:
    NumArray(vector<int>& nums) {
        for (int num : nums) {
            prefixSums.push_back(prefixSums.back() + num);
        }
    }
    
    int sumRange(int left, int right) {
        return prefixSums[right + 1] - prefixSums[left];
    }
private:
    vector<int> prefixSums = {0};
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
        unordered_map<char, int> map;
        
        for (char c : magazine) {
            ++map[c];
        }
        
        for (char c : ransomNote) {
            --map[c];
            
            if (map[c] < 0) {
                return false;
            }
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
        for (auto i = 1; i < nums.size(); i++)
        {
            nums[i] += nums[i - 1];
        }
        
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