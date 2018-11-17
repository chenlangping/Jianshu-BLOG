> Given a sorted array and a target value, return the index if the target is found. If not, return the index where it would be if it were inserted in order.
#### Example：
> Input: [1,3,5,6], 5
Output: 2

> Input: [1,3,5,6], 2
Output: 1

> Input: [1,3,5,6], 7
Output: 4

> Input: [1,3,5,6], 0
Output: 0
#### Note：
> You may assume no duplicates in the array.

#### 解释下题目：
> 给定一个排好序的数组，在给定一个数字，如果这个数字在这个数组中，返回它的下标，否则就把它插入到正确的位置，返回那个位置的下标。


## 1. 二分查找法
#### 实际耗时：5ms
```
 public int searchInsert(int[] nums, int target) {
        //二分查找法
        int small = 0;
        int big = nums.length - 1;
        int medium;
        while (big >= small) {
            medium = (small + big) / 2;
            if (target == nums[medium]) {
                return medium;
            } else if (target > nums[medium]) {
                small = medium + 1;
            } else {
                big = medium - 1;
            }
        }
        //return nums[medium] > target ? medium : medium + 1;
        return small;
    }
```
&emsp;&emsp;思路就是二分查找，找到就返回下标。没找到就应该返回small的，因为应该插入到small这个位置。当然用个判断可能更容易理解
###### 时间复杂度O(log(n))
###### 空间复杂度O(1)
---------
