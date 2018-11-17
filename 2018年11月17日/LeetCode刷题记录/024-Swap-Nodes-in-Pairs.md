> Given a linked list, swap every two adjacent nodes and return its head.
#### Example：
> Given 1->2->3->4, you should return the list as 2->1->4->3.

#### 解释下题目：
> 给定一个链表，把这个链表中的12交换一下，34交换一下……


## 1. 老老实实交换呗
#### 实际耗时：5ms
```
public ListNode swapPairs(ListNode head) {
        ListNode result = new ListNode();
        result.next = head;
        ListNode cur = result;
        while (cur.next != null && cur.next.next != null) {
            ListNode temp = cur.next;
            ListNode temp2 = cur.next.next;
            //将第一个节点接上第三个节点
            temp.next = temp2.next;
            //将第零个节点接上第二个节点
            cur.next = temp2;
            //将第二个节点接上第一个节点
            cur.next.next = temp;
            //移动当前节点
            cur = cur.next.next;
        }
        return result.next;

    }
```
&emsp;&emsp;其实这道题我做了不少时间，主要是被一个误区困住了：我以为只需要修改两个节点，就可以完成交换了，最后发现其实有问题的，因为其实如果链表是`1->2->3->4`，如果你需要交换`3`和`4`，那么其实`2`这个节点也会参与的，只要想通了这一点就不难了。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
