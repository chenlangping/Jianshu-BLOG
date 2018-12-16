> Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that duplicates appeared at most *twice* and return the new length.
Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

#### Example：
> Given nums = [1,1,1,2,2,3],
Your function should return length = 5, with the first five elements of nums being 1, 1, 2, 2 and 3 respectively.
It doesn't matter what you leave beyond the returned length.

> Given nums = [0,0,1,1,1,1,2,3,3],
Your function should return length = 7, with the first seven elements of nums being modified to 0, 0, 1, 1, 2, 3 and 3 respectively.
It doesn't matter what values are set beyond the returned length.
#### Clarification：
> Confused why the returned value is an integer but your answer is an array?
Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.

#### 解释下题目：
> 删掉一个数组中重复的元素，使重复的元素最多只有两个


## 1. 从头开始判断
#### 实际耗时：1ms
```
public int removeDuplicates(int[] nums) {
        int i = 0;
        for (int number : nums) {
            if (i < 2 || number > nums[i - 2]) {
                nums[i] = number;
                i++;
            }
        }
        return i;
    }
```
&emsp;&emsp;思路就是前两个是肯定要放进去的，然后之后因为是有序的，所以如果数字比它前两个大，那么说明是不同的，就应该赋值，否则就应该继续下去。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
