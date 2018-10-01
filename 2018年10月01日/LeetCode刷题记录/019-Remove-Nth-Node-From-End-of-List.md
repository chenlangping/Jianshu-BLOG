> Given a linked list, remove the *n*-th node from the end of list and return its head.
#### Example：
> Given linked list: 1->2->3->4->5, and n = 2.
After removing the second node from the end, the linked list becomes 1->2->3->5.
#### Note：
> Given *n* will always be valid.

#### 解释下题目：
> 删除倒数第n个链表，n确保是合法的


## 1. 走两趟
#### 实际耗时：16ms
```
public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode listNode = new ListNode(0);
        int count = 0;
        int position;
        listNode.next = head;
        ListNode node = listNode;
        //第一趟记录下一共有多少节点
        while (node.next != null) {
            count++;
            node = node.next;
        }
        node = listNode;
        position = count - n;
        count = 0;
        //第二趟找到要删掉的节点
        while (position != count) {
            node = node.next;
            count++;
        }
        //删除操作
        node.next = node.next.next;
        return listNode.next;
    }
```
&emsp;&emsp;思路就是走两趟，第一趟计算有多少个结点，然后倒数第n个就知道是正数第几个了，第二趟删除即可。
###### 时间复杂度O(2*n)   n为结点个数
###### 空间复杂度O(1)
---------  
## 2. 走一趟
#### 实际耗时：8ms
```
public ListNode removeNthFromEnd2(ListNode head, int n) {
        ListNode listNode = new ListNode(0);
        listNode.next = head;
        ListNode listNode1 = listNode;
        ListNode listNode2 = listNode;
        //先让listNode1先跑一段
        for (int i = 0; i < n; i++) {
            listNode1 = listNode1.next;
        }
        //当listNode1到达末尾的时候，listNode2正好在要删除的节点的前面一个
        while (listNode1.next != null) {
            listNode1 = listNode1.next;
            listNode2 = listNode2.next;
        }
        //删除节点操作
        listNode2.next = listNode2.next.next;

        return listNode.next;

    }
```
&emsp;&emsp;思路就是先让第一个跑n步，然后第二个和第一个一起走，那样当第一个走到末尾的时候第二个正好是要删除的节点之前的一位。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
