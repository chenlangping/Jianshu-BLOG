> Given a linked list, reverse the nodes of a linked list `k` at a time and return its modified list.
`k` is a positive integer and is less than or equal to the length of the linked list. If the number of nodes is not a multiple of `k` then left-out nodes in the end should remain as it is.
#### Example：
> Given this linked list: 1->2->3->4->5
For k = 2, you should return: 2->1->4->3->5
For k = 3, you should return: 3->2->1->4->5
#### Note：
> Only constant extra memory is allowed.
You may not alter the values in the list's nodes, only nodes itself may be changed.

#### 解释下题目：
> 只能用交换节点的方法来逆转链表


## 1. 老老实实换呗
#### 实际耗时：8ms
```
public ListNode reverseKGroup(ListNode head, int k) {
        ListNode listNode = new ListNode(0);
        ListNode start = listNode;
        listNode.next = head;
        while (true) {
            ListNode p = start;
            ListNode q;
            ListNode cur = p;
            start = p.next;
            for (int i = 0; i < k && cur != null; i++) {
                cur = cur.next;
            }
            if (cur == null) {
                break;
            }
            for (int i = 0; i < k - 1; i++) {
                q = p.next;
                p.next = q.next;
                q.next = cur.next;
                cur.next = q;
            }
        }
        return listNode.next;
    }
```
&emsp;&emsp;思路见图：
![025.png](https://upload-images.jianshu.io/upload_images/13050335-e7567f408329ec5d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
