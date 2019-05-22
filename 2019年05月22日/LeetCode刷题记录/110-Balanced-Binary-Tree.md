> Given a binary tree, determine if it is height-balanced.
For this problem, a height-balanced binary tree is defined as:
a binary tree in which the depth of the two subtrees of every node never differ by more than 1.
#### Example：
> [3,9,20,null,null,15,7]
true

> [1,2,2,3,3,null,null,4,4]
false

#### 解释下题目：
> 判断一棵树是不是平衡二叉树


## 1. 根据树的深度判断
#### 实际耗时：0ms
```
    private boolean isBanlanced = true;

    public boolean isBalanced(TreeNode root) {
        maxDepth(root);
        return isBanlanced;
    }

    public int maxDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = maxDepth(root.left);
        int right = maxDepth(root.right);
        if (Math.abs(left - right) > 1) {
            isBanlanced = false;
        }
        return 1 + Math.max(left, right);
    }
```
&emsp;&emsp;思路很简单，就是根据最深子树进行判断呗。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
