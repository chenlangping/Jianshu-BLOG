> Given an integer *n* , generate all structurally unique **BST's** (binary search trees) that store values 1 ... n.
#### Example：
> Input: 3
Output:
[
  [1,null,3,2],
  [3,2,null,1],
  [3,1,null,null,2],
  [2,1,3],
  [1,null,2,null,3]
]

#### 解释下题目：
> 打印出所有的二叉搜索树


## 1. 递归
#### 实际耗时：2ms
```
   public List<TreeNode> generateTrees(int n) {
        if (n == 0) {
            return new ArrayList<>();
        }
        return recursive(1, n);
    }

    public List<TreeNode> recursive(int start, int end) {
        List<TreeNode> res = new ArrayList<>();
        List<TreeNode> left, right;

        // 结束条件
        if (start > end) {
            res.add(null);
            return res;
        }
        if (start == end) {
            res.add(new TreeNode(start));
            return res;
        }

        //开始递归
        for (int i = start; i <= end; i++) {
            left = recursive(start, i - 1);
            right = recursive(i + 1, end);

            for (TreeNode leftNode : left) {
                for (TreeNode rightNode : right) {
                    TreeNode root = new TreeNode(i);
                    root.left = leftNode;
                    root.right = rightNode;
                    res.add(root);
                }
            }
        }
        return res;
    }
```
###### 踩过的坑：输入0的时候
&emsp;&emsp;思路跟做别的树的时候一样，肯定是用递归的。然后因为是搜索树，所以大的肯定放右边，小的放右边。也就是对于每一个点，只需要确定它**所有**左子树的集合，然后一个一个把这些子树加上去即可。而这个所有的左子树集合也就是规模一样的子问题。右边的情况也一样。

---------
