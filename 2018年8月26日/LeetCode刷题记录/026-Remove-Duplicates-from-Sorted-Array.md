> Given a sorted array *nums*, remove the duplicates [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) such that each element appears only *once* and return the new length.
Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.

#### Example：
> Given nums = [1,1,2],
Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively.
It doesn't matter what you leave beyond the returned length.

> Given nums = [0,0,1,1,1,2,2,3,3,4],
Your function should return length = 5, with the first five elements of nums being modified to 0, 1, 2, 3, and 4 respectively.
It doesn't matter what values are set beyond the returned length.
#### Clarification：
> Confused why the returned value is an integer but your answer is an array?
Note that the input array is passed in by reference, which means modification to the input array will be known to the caller as well.

Internally you can think of this:
```
// nums is passed in by reference. (i.e., without making a copy)
int len = removeDuplicates(nums);

// any modification to nums in your function would be known by the caller.
// using the length returned by your function, it prints the first len elements.
for (int i = 0; i < len; i++) {
    print(nums[i]);
}
```
#### 解释下题目：
> 返回数组中不重复的数字的个数。上面解释了一下为什么要返回不同数组的个数而不是返回处理过后的数组。不能使用额外的空间，也就是说必须在这个数组上进行处理。


## 1. 处理之后放入数组前端
#### 实际耗时：12ms
```
public int removeDuplicates(int[] nums) {
        int length = nums.length;
        if (length == 0) {
            return 0;
        }
        int count = 0;
        for (int i = 1; i < length; i++) {
            if (nums[i] != nums[count]) {
                count++;
                nums[count] = nums[i];
            }
        }
        return count + 1;
    }
```
&emsp;&emsp;思路：因为是已经排过序的数组，所以相同的数肯定是相邻的，而且是删除重复的（其实也不是删除）元素，所以最坏的情况是这个数组本身就没有重复的。思路就是处理过后就覆盖掉，因为肯定有处理后的数组<=处理前的数组，所以不用担心。
###### 时间复杂度O(n) n为数组长度
###### 空间复杂度O(1)
---------

