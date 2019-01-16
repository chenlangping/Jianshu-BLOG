> Given a sorted linked list, delete all nodes that have duplicate numbers, leaving only *distinct* numbers from the original list.
#### Example：
> Input: 1->2->3->3->4->4->5
Output: 1->2->5

> Input: 1->1->1->2->3
Output: 2->3

#### 解释下题目：
> 给定一个排好序的链表，删除其中所有的重复的数字。


## 很巧妙的方法
#### 实际耗时：1ms
```
public ListNode deleteDuplicates(ListNode head) {
        if (head == null) {
            return null;
        }
        ListNode FakeHead = new ListNode(0);
        FakeHead.next = head;
        ListNode pre = FakeHead;
        ListNode cur = head;
        while (cur != null) {
            while (cur.next != null && cur.val == cur.next.val) {
                cur = cur.next;
            }
            if (pre.next == cur) {
                //这里说明pre和cur指向的两个数是不同的，那么pre直接向下移动就好了
                pre = pre.next;
            } else {
                //这里说明cur已经经历过多个相同的数值了，那么这些数值全部不要。
                pre.next = cur.next;
            }
            cur = cur.next;
        }
        return FakeHead.next;
    }
```
###### 踩过的坑：原来我自己写的方法是有三个变量来考虑的，而且还有很多种情况。
&emsp;&emsp;思路都写在注释里了。
###### 时间复杂度O(n)  n为链表长度
###### 空间复杂度O(n)  n为链表长度
---------
