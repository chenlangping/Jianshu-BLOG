> Given a binary tree, return the bottom-up level order traversal of its nodes' values. (ie, from left to right, level by level from leaf to root).
#### Example：
> Given binary tree [3,9,20,null,null,15,7]
[
  [15,7],
  [9,20],
  [3]
]

#### 解释下题目：
> 就是把一棵二叉树用list表示


## 1. 使用队列
#### 实际耗时：2ms
```
public class Solution {
    public List<List<Integer>> levelOrderBottom(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        if (root == null) {
            return res;
        }
        Queue<TreeNode> q = new LinkedList<>();
        q.add(root);
        while (q.size() > 0) {
            List<Integer> list = new ArrayList<>();
            int size = q.size();
            for (int i = 0; i < size; i++) {
                TreeNode node = q.poll();
                list.add(node.val);
                if (node.left != null) q.add(node.left);
                if (node.right != null) q.add(node.right);
            }
            res.add(0, list);
        }
        return res;
    }
}
```
&emsp;&emsp;思路就是用队列按序做
###### 时间复杂度O(n^2)
###### 空间复杂度O(1)
---------
