## 题目

Given a 2D matrix matrix, find the sum of the elements inside the rectangle defined by its upper left corner (row1, col1) and lower right corner (row2, col2).

## 分析

动态规划，给定两个坐标，求其定位的矩形中所有元素的和。先建立一个二维数组，其中存放的是从原点到当前位置所组成的矩形中所有元素的和。通过这个数组的中所存放的信息，我们可以很容易的得到我们想要的值。过程中注意边界值的计算和各种特殊情况的计算。

## 代码

```
class NumMatrix {
public:
    NumMatrix(vector<vector<int>> matrix) {
        int m = matrix.size(), i, j;
        if (m == 0)
            return;
        int n = matrix[0].size();
        OPT.resize(m, vector<int>(n));
        OPT[0][0] = matrix[0][0];
        for (i = 1; i < m; i++)
            OPT[i][0] = OPT[i - 1][0] + matrix[i][0];
        for (i = 0; i < n; i++)
            OPT[0][i] = OPT[0][i - 1] + matrix[0][i];
        
        for (i = 1; i < m; i++)
            for (j = 1; j < n; j++)
                OPT[i][j] = OPT[i - 1][j] + OPT[i][j - 1] - OPT[i - 1][j - 1] + matrix[i][j]; 
        // for (i = 0; i < m; i++){
        //     for(j = 0; j < n; j++)
        //         cout << OPT[i][j] << ' ';
        //     cout << endl;
        // }
    
    }
    
    int sumRegion(int row1, int col1, int row2, int col2) {
        int subnum = 0;
        if (row1 == 0 && col1 == 0)
            return OPT[row2][col2];
        if (row1 == 0)
            return OPT[row2][col2] - OPT[row2][col1 - 1];
        if (col1 == 0)
            return OPT[row2][col2] - OPT[row1 - 1][col2];
        return OPT[row2][col2] - OPT[row1 - 1][col2] - OPT[row2][col1 - 1] + OPT[row1 - 1][col1 - 1];
            
    }
    
private: vector<vector<int>> OPT;
};

/**
 * Your NumMatrix object will be instantiated and called as such:
 * NumMatrix obj = new NumMatrix(matrix);
 * int param_1 = obj.sumRegion(row1,col1,row2,col2);
 */
```