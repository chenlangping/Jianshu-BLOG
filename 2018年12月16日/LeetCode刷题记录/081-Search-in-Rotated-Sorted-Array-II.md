> Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
(i.e., `[0,0,1,2,2,5,6]` might become `[2,5,6,0,0,1,2]`).
You are given a target value to search. If found in the array return `true`, otherwise return `false` .
#### Example：
> Input: nums = [2,5,6,0,0,1,2], target = 0
Output: true

> Input: nums = [2,5,6,0,0,1,2], target = 3
Output: false
#### Note：
> 

#### 解释下题目：
> 与[031](https://www.jianshu.com/p/0cbe01911fa0)一样，只是这个数组可能有重复的数字。


## 1. 和[031](https://www.jianshu.com/p/0cbe01911fa0)一样，加个重复判断即可
#### 实际耗时：0ms
```
public boolean search(int[] nums, int target) {
        int small = 0;
        int big = nums.length - 1;
        int mid;
        while (big >= small) {
            mid = (big + small) >> 1;
            if (nums[mid] == target) {
                return true;
            } else if (nums[big] == nums[small] && nums[small] == nums[mid]) {
                //如果三者在同一条直线上
                small++;
            } else if (nums[mid] >= nums[small]) {
                //此时左半部分一定是非递减的
                if (target < nums[mid] && target >= nums[small]) {
                    big = mid - 1;
                } else {
                    small = mid + 1;
                }
            } else {
                //此时在右半部分是非递减的
                if (target > nums[mid] && target <= nums[big]) {
                    small = mid + 1;
                } else {
                    big = mid - 1;
                }
            }

        }
        return false;


    }
```
###### 踩过的坑：{3,1,1} ,查找1
&emsp;&emsp;思路，和[031](https://www.jianshu.com/p/0cbe01911fa0)一样，只是加了一个判断，就是如果三者在同一条线上的时候，这个时候可以移动small或者big，其他没什么区别。稍微巧妙一点的操作是我把最后那个右半部分的判断给省略了，直接用else来代替。
###### 时间复杂度O(log n)
###### 空间复杂度O(1)
---------
