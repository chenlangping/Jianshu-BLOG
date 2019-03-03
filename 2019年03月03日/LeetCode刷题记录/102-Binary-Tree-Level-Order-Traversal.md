> Given a binary tree, return the *level order* traversal of its nodes' values. (ie, from left to right, level by level).
#### Example：
> Given binary tree [3,9,20,null,null,15,7],return its level order traversal as:
[
  [3],
  [9,20],
  [15,7]
]

#### 解释下题目：
> 对于一棵二叉树，分别获取它的每一层。


## 1. 利用队列
#### 实际耗时：2ms
```
public List<List<Integer>> levelOrder(TreeNode root) {
        List<List<Integer>> res = new ArrayList<>();
        Queue<TreeNode> queue = new LinkedList<>();

        if (null == root) {
            return res;
        }

        queue.offer(root);
        while (!queue.isEmpty()) {
            List<Integer> tmp = new ArrayList<>();
            //用一个变量来记录当前的queue的长度，这里很重要，因为queue一直在变！
            int queueLen = queue.size();
            for (int i = 0; i < queueLen; i++) {
                //对于每一个节点，我们先把它的左右加入进来，然后把它删除，相当于一层一层进行处理
                if (queue.peek().left != null) {
                    queue.offer(queue.peek().left);
                }
                if (queue.peek().right != null) {
                    queue.offer(queue.peek().right);
                }
                tmp.add(queue.poll().val);
            }
            res.add(tmp);
        }
        return res;
    }
```
###### 踩过的坑：排查了好久，我一直没用`queueLen`这个，而是用的`queue.size()`，这里有个问题就是这个队列其实是动态变化的。
&emsp;&emsp;思路就是对于一棵树如果它有子树，那就把它的子树加进队列，然后对它处理。
###### 时间复杂度O(n) n是节点数
###### 空间复杂度O(logn) n是节点数
---------
