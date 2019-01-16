> Given two binary trees, write a function to check if they are the same or not.
Two binary trees are considered the same if they are structurally identical and the nodes have the same value.
#### Example：
> Input:   [1,2,3],   [1,2,3]
Output: true

> Input:   [1,2],     [1,null,2]
Output: false


#### 解释下题目：
> 判断两个二叉树是否相等


## 1. 递归判断
#### 实际耗时：5ms
```
public boolean isSameTree(TreeNode p, TreeNode q) {
        boolean result = false;

        if (p == null && q == null) {
            //两个都为空说明是相等的
            return true;
        } else if (p == null || q == null) {
            //一个空一个非空说明不相等
            return false;
        }

        //值相同则递归判断
        if (p.val == q.val) {
            result = isSameTree(p.left, q.left) && isSameTree(p.right, q.right);
        }

        return result;
    }
```
###### 踩过的坑：把&&弄成 ||
&emsp;&emsp;思路就是递归判断呀，两棵树要一样的话，所有都得一样。
###### 时间复杂度O(n)  n是节点个数
###### 空间复杂度O(1)
---------
