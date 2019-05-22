> Given a binary tree, find its minimum depth.
The minimum depth is the number of nodes along the shortest path from the root node down to the nearest leaf node.
#### Example：
> input：[3,9,20,null,null,15,7]
output：2
#### Note：
> A leaf is a node with no children.

#### 解释下题目：
> 返回一棵二叉树到子树最小的距离


## 1. DFS
#### 实际耗时：0ms
```
    public int minDepth(TreeNode root) {
        return getMinDepth(root);
    }

    public int getMinDepth(TreeNode root) {
        if (root == null) {
            return 0;
        }
        int left = getMinDepth(root.left);
        int right = getMinDepth(root.right);
        if (left == 0) {
            return right + 1;
        }
        if (right == 0) {
            return left + 1;
        }

        int min = Math.min(left, right);
        return min + 1;
    }
```
###### 踩过的坑：[1,2] 应该输出2
&emsp;&emsp;思路很简单，就是有一点我第一次做的时候忽略了，就是如果一个节点要都没有左子树和右子树才是到达最下面了，否则还需要走，我就是错在这里
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
