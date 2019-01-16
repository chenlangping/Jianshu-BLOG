> Given a binary tree, determine if it is a valid binary search tree (BST).
Assume a BST is defined as follows:
* The left subtree of a node contains only nodes with keys **less than** the node's key.
* The right subtree of a node contains only nodes with keys **greater than** the node's key.
* Both the left and right subtrees must also be binary search trees.
#### Example：
> Input:
    2
   / \\
  1   3
Output: true

#### 解释下题目：
> 判断一棵树是否二分搜索树，二分搜索树是左子树都小于根节点，右子树全部大于根子树。


## 1. 二叉树肯定用递归的啦
#### 实际耗时：0ms
```
public boolean isValidBST(TreeNode root) {
        return isValidBST(root, Long.MIN_VALUE, Long.MAX_VALUE);
    }

    public boolean isValidBST(TreeNode root, long minVal, long maxVal) {
        if (root == null) {
            return true;
        }
        if (root.val >= maxVal || root.val <= minVal) {
            return false;
        }

        return isValidBST(root.left, minVal, root.val) && isValidBST(root.right, root.val, maxVal);
    }
```
&emsp;&emsp;思路就是递归。
###### 时间复杂度O(n) 因为每个节点都遍历一遍
###### 空间复杂度O(1)
---------
