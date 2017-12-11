## 题意

Given an unsorted array of integers, find the length of longest increasing subsequence.

For example,
Given [10, 9, 2, 5, 3, 7, 101, 18],
The longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. Note that there may be more than one LIS combination, it is only necessary for you to return the length.

## 分析

## 代码

```
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int l = nums.size(), i, mid, left, right;
        if (l == 0)
            return false;
        vector<int> sorted;
        sorted.push_back(nums[0]);
        for (i = 1; i < l; i++){
            if (nums[i] < sorted[0]){
                sorted[0] = nums[i];
                continue;
            }
            if (nums[i] > sorted[sorted.size() - 1]){
                sorted.push_back(nums[i]);
                continue;
            }
            left = 0;
            right = sorted.size();
            while (left < right){
                mid = (left + right) / 2;
                if (sorted[mid] < nums[i]){
                    left = mid + 1;
                    continue;
                }
                right = mid;
            }
            sorted[right] = nums[i]; 
        }
        return sorted.size();
    }
};
```