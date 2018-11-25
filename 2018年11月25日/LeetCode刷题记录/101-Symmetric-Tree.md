> Given a binary tree, check whether it is a mirror of itself (ie, symmetric around its center).
For example, this binary tree `[1,2,2,3,4,4,3]` is symmetric:
#### Example：
>  `[1,2,2,3,4,4,3]`
true

> `[1,2,2,null,3,null,3]`
false
#### Note：
> Bonus points if you could solve it both recursively and iteratively.

#### 解释下题目：
> 判断一棵树是不是镜像的


## 1. 递归1
#### 实际耗时：12ms
```
public boolean isSymmetric(TreeNode root) {
        if (root == null) {
            return true;
        }
        return isMirror(root.left, root.right);
    }

    public boolean isMirror(TreeNode left, TreeNode right) {
        if (left == null && right == null) {
            return true;
        } else if (left == null || right == null) {
            return false;
        } else {
            if (left.val == right.val) {
                return isMirror(left.left, right.right) && isMirror(left.right, right.left);
            } else {
                return false;
            }
        }
    }
```
###### 踩过的坑：空树
&emsp;&emsp;思路很简单，直接看就行
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------


