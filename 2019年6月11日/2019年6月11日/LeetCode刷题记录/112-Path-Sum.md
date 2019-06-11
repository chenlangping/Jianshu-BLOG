> Given a binary tree and a sum, determine if the tree has a root-to-leaf path such that adding up all the values along the path equals the given sum.
#### Example：
>   ![image.png](https://upload-images.jianshu.io/upload_images/13050335-d5789720cdde7ad6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### Note：
> A leaf is a node with no children.

#### 解释下题目：
> 找出一个从根到叶子节点的路，使这条路上的所有的数字和是sum


## 1. 递归
#### 实际耗时：1ms
```
public boolean hasPathSum(TreeNode root, int sum) {
        if (root == null) {
            return false;
        }
        if (root.left == null && root.right == null && sum - root.val == 0) {
            return true;
        }

        return hasPathSum(root.left, sum - root.val) || hasPathSum(root.right, sum - root.val);
    }
```
&emsp;&emsp;思路就是递归，简单的一匹。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
