> Given a singly linked list where elements are sorted in ascending order, convert it to a height balanced BST.
For this problem, a height-balanced binary tree is defined as a binary tree in which the depth of the two subtrees of every node never differ by more than 1.
#### Example：
> Given the sorted linked list: [-10,-3,0,5,9],One possible answer is: [0,-3,9,-10,null,5]

#### 解释下题目：
> 跟之前的[108](https://www.jianshu.com/p/31a5bd31ebfd)其实是一样的


## 1. 二分法递归做
#### 实际耗时：2ms
```
public TreeNode sortedListToBST(ListNode head) {
        List<Integer> res = new ArrayList<>();
        ListNode cur = head;
        while (cur != null) {
            res.add(cur.val);
            cur = cur.next;
        }
        return recursive(res, 0, res.size() - 1);
    }

    public TreeNode recursive(List<Integer> res, int low, int high) {
        if (low > high) {
            return null;
        }
        int mid = (low + high) >> 1;
        TreeNode root = new TreeNode(res.get(mid));
        root.left = recursive(res, low, mid - 1);
        root.right = recursive(res, mid + 1, high);
        return root;
    }
```
###### 踩过的坑：
&emsp;&emsp;思路见[108](https://www.jianshu.com/p/31a5bd31ebfd)
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------
