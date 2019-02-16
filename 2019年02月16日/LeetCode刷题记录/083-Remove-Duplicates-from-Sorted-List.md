> Given a sorted linked list, delete all duplicates such that each element appear only once.
#### Example：
> Input: 1->1->2
Output: 1->2

> Input: 1->1->2->3->3
Output: 1->2->3

#### 解释下题目：
> 给定一个排好序的链表，删除其中的重复元素


## 1. 老老实实做呗
#### 实际耗时：0ms
```
public ListNode deleteDuplicates(ListNode head) {
        ListNode current = head;
        ListNode result = current;
        if (null == current) {
            return null;
        }
        while (current.next != null) {
            if (current.val == current.next.val) {
                current.next = current.next.next;
            } else {
                current = current.next;
            }
        }
        return result;
    }
```
&emsp;&emsp;思路：就是最简单的要是跟下一个一样，就把下一个跳过去即可。
###### 时间复杂度O(n)  n为链表长度
###### 空间复杂度O(n)  n为链表长度
---------
