### 题意

Given two strings s1, s2, find the lowest ASCII sum of deleted characters to make two strings equal.

### 思路

动态规划类的题目，OPT[i][j]代表 s1[0 : i] 和 s2[0 : j]成为相同的字符串所删除的最少的字符ASCII码之和。则在第[i][j]位置得到相同的字符串，可以由下面四种情况得到：
1. 若s1[i] = s2[j],则不用做任何操作，即 OPT[i][j] = OPT[i - 1][j - 1];
2. s1[i] != s2[j], 则可以通过将两者都删除来达到状态，即 OPT[i][j] = OPT[i - 1][j - 1] + s1[i] + s2[j];
3. 可以通过OPT[i - 1][j] 的状态得到，即删去s1[i], OPT[i][j] = OPT[i - 1][j] + s1[i];
4. 同上，OPT[i][j] = OPT[i][j - 1] + s2[j];

### 代码

```
class Solution {
public:
    int minimumDeleteSum(string s1, string s2) {
        int n1 = s1.length();
        int n2 = s2.length();
        int result = 0, i, j;
        if (n1 == 0){
            for (i = 0; i < n2; i++)
                result += s2[i];
            return result;
        }
        if (n2 == 0){
            for (i = 0; i < n1; i++)
                result += s1[i];
            return result;
        }
        
        vector<vector<int>> OPT(n1 + 1, vector<int>(n2 + 1));
        OPT[0][0] = 0;
        for (i = 1; i < n1 + 1; i++)
            OPT[i][0] = OPT[i - 1][0] + s1[i - 1];
        for (i = 1; i < n2 + 1; i++)
            OPT[0][i] = OPT[0][i - 1] + s2[i - 1];
        
        for (i = 0; i < n1; i++)
            for (j = 0; j < n2; j++){
                OPT[i + 1][j + 1] = s1[i] == s2[j] ? OPT[i][j] : OPT[i][j] + s1[i] + s2[j];
                OPT[i + 1][j + 1] = min(min(OPT[i + 1][j + 1], OPT[i + 1][j] + s2[j]), OPT[i][j + 1] + s1[i]);
               //        printf("OPT[%d][%d] = %d\n", i + 1, j + 1, OPT[i + 1][j + 1]);
            }
        
        return OPT[n1][n2];
        
    }
};
``` 