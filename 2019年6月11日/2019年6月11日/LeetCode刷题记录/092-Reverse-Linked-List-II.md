> Reverse a linked list from position m to n. Do it in one-pass.
#### Example：
> Input: 1->2->3->4->5->NULL, m = 2, n = 4
Output: 1->4->3->2->5->NULL
#### Note：
> 1 ≤ m ≤ n ≤ length of list.

#### 解释下题目：
> 在一次循环中把链表指定位置倒序


## 1. 老实倒序
#### 实际耗时：3ms
```
public ListNode reverseBetween(ListNode head, int m, int n) {
        if (head == null) {
            return null;
        }
        ListNode fakeHead = new ListNode(0);
        fakeHead.next = head;
        ListNode pre = fakeHead;
        for (int i = 0; i < m - 1; i++) {
            //不停移动pre  直到pre到达要开始逆序的之前的那个数
            pre = pre.next;
        }

        //start指向将要换的那个数  nex指向start的下一个数
        ListNode start = pre.next;
        ListNode nex = start.next;

        //开始执行n-m次
        for (int i = 0; i < n - m; i++) {
            start.next = nex.next;
            nex.next = pre.next;
            pre.next = nex;
            nex = start.next;
        }

        return fakeHead.next;

    }
```
&emsp;&emsp;思路很难说，直接上个交换的顺序自己感受下：
```
1->3->2->4->5->6->7->8->9
1->4->3->2->5->6->7->8->9
1->5->4->3->2->6->7->8->9
1->6->5->4->3->2->7->8->9
1->7->6->5->4->3->2->8->9
1->8->7->6->5->4->3->2->9
```
&emsp;&emsp;可以发现的一点是，最开始要交换的这个数到最后肯定是到最后的。我一开始被这个卡住了。还有注意的是交换的斜对角线原则。就没什么问题了。
###### 时间复杂度O(m-n)
###### 空间复杂度O(1)
---------
