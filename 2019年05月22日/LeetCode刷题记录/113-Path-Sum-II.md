> Given a binary tree and a sum, find all root-to-leaf paths where each path's sum equals the given sum.
#### Example：
> input：[5,4,8,11,null,13,4,7,2,null,null,null,null,5,1]
output：
[
   [5,4,11,2],
   [5,8,4,5]
]
#### Note：
>  A leaf is a node with no children.

#### 解释下题目：
> 给定一个数字，然后找出所有的从根节点到叶子节点到根节点的和等于指定值的和。


## 1. DFS
#### 实际耗时：1ms
```
    public List<List<Integer>> pathSum(TreeNode root, int sum) {
        List<List<Integer>> finalResult = new ArrayList<>();
        List<Integer> tmpResult = new ArrayList<>();
        recursive(root, sum, finalResult, tmpResult);
        return finalResult;
    }

    public void recursive(TreeNode root, int sum, List<List<Integer>> finalResult
            , List<Integer> tmpResult) {
        if (null == root) {
            return;
        }
        tmpResult.add(root.val);
        if (root.left == null && root.right == null && sum == root.val) {
            finalResult.add(new ArrayList<>(tmpResult));
            tmpResult.remove(tmpResult.size() - 1);
            return;
        } else {
            recursive(root.left, sum - root.val, finalResult, tmpResult);
            recursive(root.right, sum - root.val, finalResult, tmpResult);
        }
        tmpResult.remove(tmpResult.size() - 1);
    }
```
&emsp;&emsp;需要注意的是就是在临时存的链表中删除值这个操作。
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------
