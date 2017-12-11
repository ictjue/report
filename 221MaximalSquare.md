## 题目

Given a 2D binary matrix filled with 0's and 1's, find the largest square containing only 1's and return its area.

## 分析

给定一个矩阵，求其中全1子方阵的最大面积。
动态规划，设OPT[i][j]为以此点为右下角的全1方阵的最大数目。通过对OPT[i - 1][j - 1]和第i行和第j列的分析，可知，当前点的最大子方阵的边长为OPT[i - 1][j]、OPT[i][j - 1]h和OPT[i - 1][j - 1]中边长较小值再加1。

## 代码

```
class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int i, j, m = matrix.size(), n, temp;
         if (m == 0)
            return 0;
        n = matrix[0].size();
        vector<vector<int>> OPT(m, vector<int>(n));
        for (i = 0; i < m; i++)
            OPT[i][0] = matrix[i][0] == '1' ? 1 : 0;
        for (j = 0; j < n; j++)
            OPT[0][j] = matrix[0][j] == '1' ? 1 : 0;
        for (i = 1; i < m; i++)
            for(j = 0; j < n; j++){
                if (matrix[i][j] == '0'){
                    OPT[i][j] = 0;
                    continue;
                }
                temp = sqrt(min(OPT[i][j - 1],min(OPT[i - 1][j], OPT[i - 1][j - 1])));
                OPT[i][j] = (temp + 1) * (temp + 1);
            }
        temp = OPT[0][0];
        for (i = 0; i < m; i++)
            for (j = 0; j < n; j++)
                temp = max(temp, OPT[i][j]);
        return temp;
        
    }
};
```