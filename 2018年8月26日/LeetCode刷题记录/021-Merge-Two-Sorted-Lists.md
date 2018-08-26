> Merge two sorted linked lists and return it as a new list. The new list should be made by splicing together the nodes of the first two lists.
#### Example：
> Input: 1->2->4, 1->3->4
Output: 1->1->2->3->4->4

#### 解释下题目：
> 将两个排好序的链表合成一个


## 1. 老老实实做
#### 实际耗时：9ms
```
public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        //如果其中一个已经有序，那么直接返回另外一个就行了
        if (l1 == null) return l2;
        if (l2 == null) return l1;

        ListNode head = new ListNode(0);
        ListNode current = head;
        //谁长就把谁放进去
        while (l1 != null && l2 != null) {
            if (l1.val > l2.val) {
                current.next = l2;
                l2 = l2.next;
            } else {
                current.next = l1;
                l1 = l1.next;
            }
            current = current.next;
        }

        //此时有一个已经是Null了
        while (l1 != null) {
            current.next = l1;
            l1 = l1.next;
            current = current.next;
        }

        while (l2 != null) {
            current.next = l2;
            l2 = l2.next;
            current = current.next;
        }

        return head.next;
    }
```
&emsp;&emsp;思路：每个链表取出最前面的进行比较，把较小的放进链表，直到一个变成空，接下来把另外一个不是空的全部放进去即可
###### 时间复杂度O(n+m) n和m分别为两个链表的长度
###### 空间复杂度O(1)
---------

