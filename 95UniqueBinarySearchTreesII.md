## 题目

Given an integer n, generate all structurally unique BST's (binary search trees) that store values 1...n.

## 分析

这题是96题的升级版，从求树的数目发展成将树都表示出来。不过思想都是差不多的，要用到动态规划的思想。在每个选定根节点的时候，先将左右子树计算出来，然后对左右子树每种情况进行组合。左右子树的求解就得利用动态规划。

## 代码

```
/** 
 * Definition for binary tree 
 * struct TreeNode { 
 *     int val; 
 *     TreeNode *left; 
 *     TreeNode *right; 
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {} 
 * }; 
 */  
class Solution {  
public:  
    vector<TreeNode *> generateTrees(int n) { 
        if (n == 0){
            vector<TreeNode*> result;
            return result;
        }
       return createTree(1,n);  
    }  
      
    vector<TreeNode *> createTree(int start, int end)  
    {  
        vector<TreeNode *> results;  
        if(start>end)  
        {  
            results.push_back(NULL);  
            return results;  
        }  
          
        for(int k=start;k<=end;k++)  
        {  
            vector<TreeNode *> left = createTree(start,k-1);  
            vector<TreeNode *> right = createTree(k+1,end);  
            for(int i=0;i<left.size();i++)  
            {  
                for(int j=0;j<right.size();j++)  
                {  
                    TreeNode * root = new TreeNode(k);  
                    root->left = left[i];  
                    root->right = right[j];  
                    results.push_back(root);  
                }  
            }  
        }  
        return results;  
    }  
};  
```