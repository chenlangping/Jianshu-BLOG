> Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position. Return the max sliding window.
#### Example：
> Input: nums = [1,3,-1,-3,5,3,6,7], and k = 3
Output: [3,3,5,5,6,7] 
Explanation: 

| Window position      |          Max|
|:--|:--
|[1  3  -1] -3  5  3  6  7    |   3  
| 1 [3  -1  -3] 5  3  6  7    |   3
| 1  3 [-1  -3  5] 3  6  7   |    5
| 1  3  -1 [-3  5  3] 6  7   |    5
| 1  3  -1  -3 [5  3  6] 7    |   6
 |1  3  -1  -3  5 [3  6  7]  |    7
#### Note：
> You may assume k is always valid, 1 ≤ k ≤ input array's size for non-empty array.

#### 解释下题目：
> 找出每个滑动窗口中最大的那个数，并且把这个数放入数组中返回。


## 1. 使用队列
#### 实际耗时：13ms
```java
public int[] maxSlidingWindow(int[] nums, int k) {
        if (nums.length == 0) {
            return new int[0];
        }
        int[] res = new int[nums.length - k + 1];
        Deque<Integer> queue = new LinkedList<>();

        for (int i = 0; i < nums.length; i++) {
            //删除已经确定不在滑动窗口中的元素
            if (!queue.isEmpty() && queue.peek() < i - k + 1) {
                queue.removeFirst();
            }

            //删除一定不可能是最大的
            while (!queue.isEmpty() && nums[i] >= nums[queue.getLast()]) {
                queue.removeLast();
            }

            //放入最新的一个
            queue.add(i);

            //写入最终的数组
            if (i - k + 1 >= 0) {
                res[i - k + 1] = nums[queue.getFirst()];
            }

        }

        return res;
    }
```
&emsp;&emsp;其实一开始我看了一眼题目，以为是找出最大的滑动窗口，于是..... 接下来认真看了题目，第一反应是那我对每个滑动窗口求个最大值，那么复杂度就是O(klogk * (n-k) )。接下来想的是滑动窗口每次都是滑出一个，滑进来一个，那我可不可以维护一个有顺序的数组，这样只需要把滑进来的元素移动到合适的位置就可以保证有序的数组了，就相当于把之前的排序优化成了二分查找法。但是后来去看了解法，确实很不错。
 &emsp;&emsp; 一开始我想的是存的是数组的值，但是解法里面存的却是数组的下标，核心思想是，每个数字其实能够“覆盖”它左边的k-1和右边k-1个数字。也就是每当这个数字进来的时候，比它小的统统可以滚蛋了，因为它的存在能够确定那些比它小的数字已经不可能是最大的了。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
