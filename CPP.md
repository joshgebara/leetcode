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
        int minutes = 0;
        int mask = 0;
        for (int i = garbage.size() - 1; i >= 0; i--)
        {
            minutes += garbage[i].size();

            for (const auto& c : garbage[i])
            {
                if (c == 'M') mask |= 1;
                if (c == 'P') mask |= (1 << 1);
                if (c == 'G') mask |= (1 << 2);
            }

            int trucks = __builtin_popcount(mask);
            if (i - 1 >= 0)
                minutes += trucks * travel[i - 1];
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
        // Guass Formula
        int totalSum = n * (n + 1) / 2;
        
        int pivotCandidate = sqrt(totalSum);
        if (pivotCandidate * pivotCandidate == totalSum) {
            return pivotCandidate;
        }

        return -1;
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
        int maxAltitude = 0;
        int altitude = 0;

        for (const auto& g : gain) {
            altitude += g;
            maxAltitude = max(maxAltitude, altitude);
        }

        return maxAltitude;
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