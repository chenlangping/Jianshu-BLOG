> Given a linked list, rotate the list to the right by *k* places, where k is non-negative.
#### Example：
> Input: 1->2->3->4->5->NULL, k = 2
Output: 4->5->1->2->3->NULL
Explanation:
rotate 1 steps to the right: 5->1->2->3->4->NULL
rotate 2 steps to the right: 4->5->1->2->3->NULL

> Input: 0->1->2->NULL, k = 4
Output: 2->0->1->NULL
Explanation:
rotate 1 steps to the right: 2->0->1->NULL
rotate 2 steps to the right: 1->2->0->NULL
rotate 3 steps to the right: 0->1->2->NULL
rotate 4 steps to the right: 2->0->1->NULL

#### 解释下题目：
> 根据K的数值来对链表进行移动


## 1. 计算出要断开的结点
#### 实际耗时：13ms
```
public ListNode rotateRight(ListNode head, int k) {
        if (head == null) {
            return null;
        }
        if (k == 0) {
            return head;
        }
        ListNode p = head;
        ListNode q = head;
        ListNode tmp;
        int len = getListNodeLength(head);
        int pace = k % len;
        for (int i = 0; i < len - pace - 1; i++) {
            p = p.next;
        }
        while (q.next != null) {
            q = q.next;
        }
        q.next = head;
        tmp = p.next;
        p.next = null;
        return tmp;
    }

    public static int getListNodeLength(ListNode listNode) {
        int length = 0;
        if (listNode == null) {
            return 0;
        }
        while (listNode != null) {
            length++;
            listNode = listNode.next;
        }
        return length;
    }
```
###### 踩过的坑：空链表和k=0
&emsp;&emsp;思路是首先得到链表的长度，然后根据K的大小算出要断开的点，然后就是执行断开操作
###### 时间复杂度O(n)
###### 空间复杂度O()
---------
