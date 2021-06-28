## Leetcode C++ Solutions

## 53. Maximum Subarray
```cpp
// Kadane's Algorithm
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if (nums.size() == 0)
        {
            return 0;
        }
        
        int globalMax = nums[0];
        int localMax = nums[0];
        
        for (auto i = 1; i < nums.size(); ++i)
        {
            localMax = max(localMax + nums[i], nums[i]);
            globalMax = max(localMax, globalMax);
        }
        
        return globalMax; 
    }
};
```

