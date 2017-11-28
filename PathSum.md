求一棵二叉树上是否存在一条路径，路径上结点的值的和与给定数相同。

一条路径是指从根结点到叶子结点，这个概念要牢记。我们可以从根节点后开始，假设存在一条满足条件的路径，那么他的儿子结点中必须存在在一个，使其满足sum - root的值。

迭代的最深层为判断一个点是否为叶子结点，同时叶子结点的值为sum，此时可以返回true。
其余情况返回false。
代码：
```
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    bool hasPathSum(TreeNode* root, int sum) {
        if (root == NULL)
            return false;
        if (root->left == NULL && root->right == NULL && sum == root->val)
            return true;
        return (hasPathSum(root->left, sum - root->val) || hasPathSum(root->right, sum - root->val));
    }
};
```