## 题目

Given a set of distinct positive integers, find the largest subset such that every pair (Si, Sj) of elements in this subset satisfies: Si % Sj = 0 or Sj % Si = 0.

If there are multiple solutions, return any subset is fine.

## 分析

先将给定的数组进行由小到大的排序。然后动态规划：
OPT[i]表示，包含当前元素的最大可除子集的大小。则
OPT[i] = max(OPT[j] + 1 if nums[i] % nums[j] == 0), i < j;
另建一个响亮表示可以整除当前值的数的索引值，最后通过回溯可以得到整个子集。

## 代码

```
class Solution {
public:
    vector<int> largestDivisibleSubset(vector<int>& nums) {
        int l = nums.size(), i, j, maxpos, maxnum = 0;
        vector<int> OPT(l);
        vector<int> pos(l);
        if (l == 0)
            return OPT;
        sort(nums.begin(), nums.end());
        OPT[0] = 1;
        pos[0] = -1;
        for(i = 1; i < l; i++){
            pos[i] = -1;
            OPT[i] = 1;
            for (j = i - 1; j >= 0; j--){
                if (nums[i] % nums[j] == 0 && OPT[j] >= OPT[i]){
                    OPT[i] = OPT[j] + 1;
                    pos[i] = j;
                }
            }
        //    cout << OPT[i] <<endl;
        }
        maxpos = 0;
        for (i = 0; i < l; i++)
            if (OPT[i] > maxnum){
                maxpos = i;
                maxnum = OPT[i];
            }
        vector<int> result(maxnum);
        while (maxpos >= 0){
            result[--maxnum] = nums[maxpos];
            maxpos = pos[maxpos];
        }
        return result;
    }
};
```