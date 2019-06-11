> Given a singly linked list, group all odd nodes together followed by the even nodes. Please note here we are talking about the node number and not the value in the nodes.
You should try to do it in place. The program should run in O(1) space complexity and O(nodes) time complexity.

#### Example：

> Input: 1->2->3->4->5->NULL
Output: 1->3->5->2->4->NULL

> Input: 2->1->3->5->6->4->7->NULL
Output: 2->3->6->7->1->5->4->NULL

#### Note：

> The relative order inside both the even and odd groups should remain as it was in the input.
The first node is considered odd, the second node even and so on ...

#### 解释下题目：

> 给定一个单项链表，然后将它奇数位置的排在前面，偶数位置的排在后面，注意是位置，而不是里面的值

## 1. 那就分开排序呗

#### 实际耗时：xxms

```java
public ListNode oddEvenList(ListNode head) {
        ListNode cur = head;
        ListNode odd = new ListNode(0);
        ListNode p1 = odd;
        ListNode even = new ListNode(0);
        ListNode p2 = even;
        int count = 0;
        while (cur != null) {
            count++;
            if (count % 2 == 1) {
                odd.next = cur;
                odd = odd.next;
            } else {
                even.next = cur;
                even = even.next;
            }
            cur = cur.next;
        }
        odd.next = p2.next;
        even.next = null;
        return p1.next;

    }
```

&emsp;&emsp;其实链表只要额外在创建一个头部，然后移动到对应的位置之后，修改指向的下一个链表节点就行了。所以这里额外创建了两个头部，然后去遍历待处理的链表，并且利用计数判断位置，并且加到对应的“新”链表中，最后把奇偶组装成新的链表返回即可。

###### 时间复杂度O(n)

###### 空间复杂度O(1)

------

