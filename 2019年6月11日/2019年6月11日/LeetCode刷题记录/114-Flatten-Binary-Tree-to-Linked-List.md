> Given a binary tree, flatten it to a linked list in-place.
#### Example：
> 略

#### 解释下题目：
> 给定一棵树，把它掰直。


## 1. 递归
#### 实际耗时：6ms
```
private TreeNode prev = null;

    public void flatten(TreeNode root) {
        if (root == null) {
            return;
        }
        //为什么是先右子树后左子树？因为这是从叶子开始构造的，而如果是从根开始是先左子树而后右子树的
        flatten(root.right);
        flatten(root.left);
        root.right = prev;
        root.left = null;
        prev = root;
    }
```
&emsp;&emsp;一拿到这题我首先是先到宽度优先的算法把这个树的值存下来，然后再构造成树，后来去发现了很简单的递归方法，真的很秀。
###### 时间复杂度O(n^2)
###### 空间复杂度O(1)
---------
