> Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
(i.e., `[0,1,2,4,5,6,7]` might become `[4,5,6,7,0,1,2]`).
You are given a target value to search. If found in the array return its index, otherwise return `-1`.
You may assume no duplicate exists in the array.
Your algorithm's runtime complexity must be in the order of O(log *n*).
#### Example：
> Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4

> Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1

#### 解释下题目：
> 给定一个无重复元素的有序数组，然后把这个数组一分为二，后面的接在前面的前面，然后判断target是否在这个数组中


## 1. 分层次判断
#### 实际耗时：11ms
```
public int search(int[] nums, int target) {
        int small = 0;
        int big = nums.length - 1;
        int mid;

        while (big >= small) {
            //除以2用移位操作
            mid = (big + small) >> 1;
            if (nums[mid] == target) {
                return mid;
            } else if (nums[small] <= nums[mid]) {
                //此时左半部分一定是递增的，注意等号的重要性，因为除以二可能会"向前移"
                if (target < nums[mid] && target >= nums[small]) {
                    big = mid - 1;
                } else {
                    small = mid + 1;
                }
            } else {
                //此时在mid的右边是递增的
                if (target > nums[mid] && target <= nums[big]) {
                    small = mid + 1;
                } else {
                    big = mid - 1;
                }
            }

        }
        return -1;


    }
```
###### 踩过的坑：{3，1} 查找1 ，就是因为缺了个等号
&emsp;&emsp;思路就是这种数组肯定有一半是递增的，所以只需要找到那另一半然后判断target是否属于递增的这一部分，如果是，那就把范围缩小到这递增的部分。画一个草图可以很容易看出来，大体思路就是我只需要确定好缩小的方向即可，其余都不需要操心。
###### 时间复杂度O(log n)
###### 空间复杂度O(1)
---------
## 2.还原法
#### 实际耗时：8ms
```
public int search2(int[] nums, int target) {
        int small = 0;
        int big = nums.length - 1;
        //第一步利用二分法找到最小的那个数
        while (small < big) {
            //除以2用移位操作
            int mid = (small + big) >> 1;
            if (nums[mid] > nums[big]) {
                small = mid + 1;
            } else {
                big = mid;
            }
        }
        //此时的small是数组中最小的那个数的下标

        //第二步将找到的最小的那个数视为"起点",进行正常的二分法查找
        int start = small;
        small = 0;
        big = nums.length - 1;
        //正常操作
        while (small <= big) {
            int mid = (small + big) >> 1;
            int realMid = (mid + start) % nums.length;
            if (nums[realMid] == target) {
                return realMid;
            }
            if (nums[realMid] < target) {
                small = mid + 1;
            } else {
                big = mid - 1;
            }
        }
        return -1;

    }
```
&emsp;&emsp;思路：你不是把数组切了一刀然后把大的一部分移动到前面去了嘛，那我就反其道而行之，找到最小的那个数，将其“视作”数组的起点，然后进行操作。很好理解且操作起来很快，不需要那么多判断操作。
###### 时间复杂度O(log n)
###### 空间复杂度O(1)
---------
