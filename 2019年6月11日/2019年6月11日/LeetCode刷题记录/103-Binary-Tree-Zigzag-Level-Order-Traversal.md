> Given a binary tree, return the *zigzag level order* traversal of its nodes' values. (ie, from left to right, then right to left for the next level and alternate between).
#### Example：
> Given binary tree [3,9,20,null,null,15,7]
[
  [3],
  [20,9],
  [15,7]
]
#### Note：
> 

#### 解释下题目：
> 给定一棵二叉树，按照广度优先的算法螺旋打印。


## 1. 队列的方法
#### 实际耗时：1ms
```
   public List<List<Integer>> zigzagLevelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        Deque<TreeNode> queue = new LinkedList<>();
        boolean turn = true;
        if (root == null) {
            return res;
        }
        queue.offer(root);
        while (!queue.isEmpty()) {
            if (turn) {
                //从左到右
                turn = false;
                List<Integer> tmp = new ArrayList<>();
                int queueLen = queue.size();
                for (int i = 0; i < queueLen; i++) {
                    if (queue.getFirst().left != null) {
                        queue.addLast(queue.getFirst().left);
                    }
                    if (queue.getFirst().right != null) {
                        queue.addLast(queue.getFirst().right);
                    }
                    tmp.add(queue.removeFirst().val);
                }
                res.add(tmp);
            } else {
                //从右到左
                turn = true;
                List<Integer> tmp = new ArrayList<>();
                int queueLen = queue.size();
                for (int i = 0; i < queueLen; i++) {
                    if (queue.getLast().right != null) {
                        queue.addFirst(queue.getLast().right);
                    }
                    if (queue.getLast().left != null) {
                        queue.addFirst(queue.getLast().left);
                    }
                    tmp.add(queue.removeLast().val);
                }
                res.add(tmp);
            }

        }
        return res;
    }
```
&emsp;&emsp;思路跟之前的[102](https://www.jianshu.com/p/2f6e500c4d06)差不多，只是多了一个螺旋的输出条件，那就需要对队列稍微修改一下。不算难
###### 时间复杂度O(n) n为二叉树的节点个数
###### 空间复杂度O(logn)
---------
