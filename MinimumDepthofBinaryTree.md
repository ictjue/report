求一棵二叉树的最小深度。
一棵二叉树的最小深度的为从根节点到所有叶子结点中的路径长度的最短值。
可以使用递归的方式实现。需要有判断叶子结点的程序语句，并且这是递归到最深处开始回溯的点。叶子结点为其结点不为空并且两个子节点都为空。其深度为1。
函数的返回值代表的从参数列表中的根节点到所有叶子结点中的路径的最小值。其值的可以由两个儿子结点的最小深度中的较小值加一得到。在这里要注意，如果此函数对空树的返回值为零，则当树的某个儿子为空的时候，得到的函数值会为1（0 + 1），但是此条路径并不符合题设要求（空结点不是叶子结点）。
源代码：
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
    int minDepth(TreeNode* root) {
        int a, b;
        if (root == NULL)
            return 0;
        if (root->left == NULL && root->right == NULL)
            return 1;
        a = minDepth(root->left);
        b = minDepth(root->right);
        if (a == 0)
            return b + 1;
        if (b == 0)
            return a + 1;
        return (a > b) ? b + 1 : a + 1;
    }
};
```