# **Morris中序遍历**

Morris遍历算法是另一种遍历二叉树的方法，它能将非递归的中序遍历空间复杂度降为*O*(1)。

Morris遍历算法整体步骤如下（假设当前遍历到的节点为 **x** ）：

1. 如果 **x** 无左孩子，先将 **x** 的值加入答案数组，再访问 **x** 的右孩子，即 `x = x.right` 。
2. 如果 **x** 有左孩子，则找到 **x** 左子树上最右的节点（**即左子树中序遍历的最后一个节点，x 在中序遍历中的前驱节点**），我们记为 *predecessor* 。根据 *predecessor* 的右孩子是否为空，进行如下操作。
    + 如果 *predecessor* 的右孩子为空，则将其右孩子指向 **x** ，然后访问 **x** 的左孩子，即 `x = x.left` 。
    + 如果 *predecessor* 的右孩子不为空，则此时其右孩子指向 **x** ，说明我们已经遍历完 **x** 的左子树，我们将 **x** 的值加入答案数组，然后访问 **x** 的右孩子，即 `x = x.right` 。
3. 重复上述操作，直至访问完整棵树。

```c++
class Solution {
public:
    vector<int> inorderTraversal(TreeNode* root) {
        vector<int> res;
        TreeNode *predecessor = nullptr;

        while (root != nullptr) {
            if (root->left != nullptr) {
                // predecessor 节点就是当前 root 节点向左走一步，然后一直向右走至无法走为止
                predecessor = root->left;
                while (predecessor->right != nullptr && predecessor->right != root) {
                    predecessor = predecessor->right;
                }
                
                // 让 predecessor 的右指针指向 root，继续遍历左子树
                if (predecessor->right == nullptr) {
                    predecessor->right = root;
                    root = root->left;
                }
                // 说明左子树已经访问完了，我们需要断开链接
                else {
                    res.push_back(root->val);
                    predecessor->right = nullptr;
                    root = root->right;
                }
            }
            // 如果没有左孩子，则直接访问右孩子
            else {
                res.push_back(root->val);
                root = root->right;
            }
        }
        return res;
    }
};
```
作者：LeetCode-Solution
链接：https://leetcode-cn.com/problems/binary-tree-inorder-traversal/solution/er-cha-shu-de-zhong-xu-bian-li-by-leetcode-solutio/
来源：力扣（LeetCode）