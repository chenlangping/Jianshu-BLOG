> Given a binary tree, return the *inorder* traversal of its nodes' values.
#### Example：
> Input: [1,null,2,3]
   1
     \\
     2
    /
   3

Output: [1,3,2]

#### 解释下题目：
> 就是先遍历左子树，然后遍历根节点，最后遍历右子树。。递归简直不要太简单


## 1. 递归
#### 实际耗时：0ms
```
public List<Integer> inorderTraversal(TreeNode root) {
        List<Integer> res = new ArrayList<>();
        inorder(root, res);
        return res;
    }

    private void inorder(TreeNode cur, List<Integer> list) {
        if (cur == null) {
            return;
        } else {
            inorder(cur.left, list);
            list.add(cur.val);
            inorder(cur.right, list);
        }
    }
```
&emsp;&emsp;这还要思路？
###### 时间复杂度O(n)
###### 空间复杂度O(logn)
---------
