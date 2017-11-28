平衡二叉树：它是一棵空树或者左子树和右子树的高度值之差小于1，并且左子树和右子树都是一棵平衡二叉树。
由平衡二叉树的定义来看，很直观的这道题可以使用递归的思想，然后需要一个求树的高度的方法。
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
    bool isBalanced(TreeNode* root) {
        int a, b;
        if (root == NULL)
            return true;
        if (abs(height(root->left) - height(root->right)) > 1)
            return false;
        return (isBalanced(root->left) && isBalanced(root->right));
    }
    
    int height(TreeNode* root){
        int a, b;
        if (root == NULL)
            return 0;
        a = height(root->left);
        b = height(root->right);
        return (a > b) ? a + 1: b + 1;
    }
};
```

在计算高度的时候，会由树的根节点开始计算，当没到一个节点的时候，isBalance方法会调用height方法，由于是从上到下，会有一些重复计算的部分。这也是使用递归方法的一个弊端。为了解决这个问题，可以像DP的方法那样，中间记录计算过的节点的高度。