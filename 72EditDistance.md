## 题意

Given two words word1 and word2, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.)

You have the following 3 operations permitted on a word:

a) Insert a character
b) Delete a character
c) Replace a character

## 分析

求从字符串a到字符串b的最短的编辑距离，三种操作，插入一个字符，删除一个字符，替换一个字符，三种操作的代价均为1。
可以看出此题使用动态规划的方式做比价容易。
OPT[i][j]表示：a的前i个位置与b的前j个位置匹配成功所需要的最少的编辑次数。由于我们能进行三种操作，所以OPT[i][j]可以由三种情况来得到：
1. 由OPT[i - 1][j - 1]来得到，若a[i]与b[j]相同，则两个相等，若不同，则可以进行替换操作，同时代价加一；
2. 由OPT[i - 1][j]来得到。需要将i位置的字符删除，同样代价加一；
3. 由OPT[i][j - 1]来得到。需要在i位置插入字符b[j],同样代价加一。

为了方便变成计算，给这个二维向量添加边界,即添加位置0，表示位置在字符串之前。

## 代码

```
class Solution {
public:
    int minDistance(string word1, string word2) {
        int len1 = word1.size(), len2 = word2.size();
        int i, j;
        vector<vector<int>> OPT(len1 + 1, vector<int>(len2 + 1));
        OPT[0][0] = 0;
        for (i = 1; i <= len2; i++){
            OPT[0][i] = OPT[0][i - 1] + 1;
            // printf("%d,%d = %d\n", 0 , i, OPT[0][i]);
        }
        for (i = 1; i <=len1; i++){
            OPT[i][0] = OPT[i - 1][0] + 1;
            // printf("%d,%d = %d\n", i , 0, OPT[i][0]);
        }
        for (i = 1; i <= len1; i++)
            for (j = 1; j <= len2; j++){
                OPT[i][j] = min(min(OPT[i - 1][j], OPT[i][j - 1]) + 1, OPT[i - 1][j - 1] + (word1[i - 1] == word2[j - 1] ? 0 : 1));
                // printf("%d,%d = %d\n", i , j, OPT[i][j]);
            }
        return OPT[len1][len2];
    }
};
```
