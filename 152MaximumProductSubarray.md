## 题意

Find the contiguous subarray within an array (containing at least one number) which has the largest product.

For example, given the array [2,3,-2,4],
the contiguous subarray [2,3] has the largest product = 6.

## 分析

此题不同于相加，可知此题用动态规划的方式比较容易解。比较特殊的是此题中的当前最优怎么得到。若是求最大的相加的和。则最优子结构的方程可以很容易的就写出来了。相乘的情况来说，因为有负负得正这个规则，所以当前最优可以通过前一步的当前最优与本位数相乘得到。也可以与前一步最小的负数与本位相乘得到（若本位为负数）。到这里，我们增加一个最小数的信息，就可以解决问题了。
OPT[i] -- 表示以第i个数为结尾的相乘值最大子串的最大值；
absOPT[i] -- 表示以第i个数为结尾的相乘值最小（为负数）子串的最小值；
则 
absOPT[i] = min(absOPT[i - 1] * nums[i], OPT[i - 1] * nums[i], nums[i]);
OPT[i] = max(absOPT[i - 1] * nums[i], OPT[i - 1] * nums[i], nums[i]);
最后遍历OPT[i]求得最大值即可。

## 代码

```
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        int l = nums.size();
        int i, result;
        vector<int> OPT(l);
        vector<int> absOPT(l);
        if (l == 0)
            return 0;
        OPT[0] = nums[0];
        result = OPT[0];
        absOPT[0] = nums[0];
        for (i = 1; i < l; i++){
            absOPT[i] = min(min(OPT[i - 1] * nums[i], absOPT[i - 1] * nums[i]), nums[i]);
            OPT[i] = max(max(OPT[i - 1] * nums[i], nums[i]), absOPT[i - 1] * nums[i]);
        }
        for (i = 0; i < l; i++)
            result = max(result, OPT[i]);
        return result;
    }
};
```