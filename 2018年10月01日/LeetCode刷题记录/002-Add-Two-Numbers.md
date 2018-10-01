> You are given two **non-empty** linked lists representing two non-negative integers. The digits are stored in **reverse order** and each of their nodes contain a single digit. Add the two numbers and return it as a linked list.
You may assume the two numbers do not contain any leading zero, except the number 0 itself.
#### Example：
> Input: (2 -> 4 -> 3) + (5 -> 6 -> 4)
Output: 7 -> 0 -> 8
Explanation: 342 + 465 = 807.

#### 解释下题目：
> 给定两个链表，这两个链表代表的是两个数字，然后把它们加起来。


## 1. 老老实实相加
#### 实际耗时：29ms
```
public ListNode addTwoNumbers(ListNode l1, ListNode l2) {
        ListNode node = new ListNode(0);
        ListNode n1 = l1;
        ListNode n2 = l2;
        ListNode result = node;
        //分别用n1指向l1，n2指向l2，并用result指向最后的答案
        int sum = 0;
        while (n1 != null || n2 != null) {
            sum /= 10;
            if (n1 != null) {
                sum += n1.val;
                n1 = n1.next;
            }
            if (n2 != null) {
                sum += n2.val;
                n2 = n2.next;
            }
            result.next = new ListNode(sum % 10);
            result = result.next;
        }

        //如果到了最后两位相加还是超过了10，最后再补一个“1”
        if (sum / 10 != 0) result.next = new ListNode(1);
        return node.next;

    }
```
&emsp;&emsp;思路就是简单判断下两串数字是否到头了，如果没有就把对应的位置相加即可。要是有一串走完了，那么只需要对另外一串不停加上去即可。最后需要注意的时候可能产生额外的一位进位。
###### 时间复杂度O(max(m,n))  m和n分别为两个链表的长度
###### 空间复杂度O(max(m,n)) m和n分别为两个链表的长度
---------
