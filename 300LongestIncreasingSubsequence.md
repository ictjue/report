## 题意

Given an unsorted array of integers, find the length of longest increasing subsequence.

For example,
Given [10, 9, 2, 5, 3, 7, 101, 18],
The longest increasing subsequence is [2, 3, 7, 101], therefore the length is 4. Note that there may be more than one LIS combination, it is only necessary for you to return the length.

## 分析

### 普通动态规划解法：
 
OPT[i] 表示以i结尾的最长递增子串的长度。则
OPT[i] = max(OPT[j] + 1, if num[j] < nums[i], j < i)
这种做法的复杂度为O(n</sup>2</sup>)

### 优化的方法

此种方法将速度优化到O(nlogn)，主要是利用二分查找的方法。
思路是：我们先建立一个数组sorted,将首元素先放进去，然后比较之后的元素，如果遍历到了新元素比sorted中的首元素还小的话，将其首元素替换为此元素；如果遍历到的新元素比sorted数组中的末元素还大的话，将其添加到数组的末尾；若遍历到的新元素比数组的首元素大，比尾元素小，此时用二分查找的方法找到第一个不小于次新元素的位置，覆盖掉位置上原来的数字。以此类推遍历完整个数组，此时sorted数组的长度就是我们要求的长度（但此时数组中的元素不是符合题设条件的所有元素，只是长度相等而已）。
分析一下原理。如果遍历到了小的元素，将其替换为首元素，因为以后碰到的元素只要比它大就可以构成递增的关系；如果碰到比sorted中所有元素都大元素，肯定会要将其添加到递增序列中。那么其他情况呢？若碰到其他情况的元素，将其替换的原因是：往下走可能会碰见更长的序列，替换是为了做好准备，万一碰到比替换前小但比替换后大的元素，如果替换了，就可以将其加入到sorted中去了，从sorted的头到末尾表示的当前最长。若按照这种更新方法，若以替换的方式将新元素填入到sorted中表示是我们存储的潜力股，若新元素替换了sorted中的最大的元素，这表示后面有一个新的序列将追上了原先最长的序列。（····自己都有点不太理解）。总之是个很巧妙的方法。在当前最优解上进行更新，并且保留了该保留的信息。

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