> Invert a binary tree.
#### Example：
> no example

#### 解释下题目：
> 交换左右子树


## 1. 交换左右子树即可
#### 实际耗时：0ms
```
public TreeNode invertTree(TreeNode root) {
        if (root != null) {
            TreeNode left = root.left;
            TreeNode right = root.right;
            root.left = right;
            root.right = left;
            invertTree(left);
            invertTree(right);
        }
        return root;
    }
```
&emsp;&emsp;思路：老老实实换呗。
###### 时间复杂度O(n) n为节点数
###### 空间复杂度O(1)
---------
