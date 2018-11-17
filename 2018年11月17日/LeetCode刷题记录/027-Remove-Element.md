> Given an array *nums* and a value *val*, remove all instances of that value [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm) and return the new length.
Do not allocate extra space for another array, you must do this by **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** with O(1) extra memory.
The order of elements can be changed. It doesn't matter what you leave beyond the new length.

#### Example：
> Given nums = [3,2,2,3], val = 3,
Your function should return length = 2, with the first two elements of nums being 2.
It doesn't matter what you leave beyond the returned length.

> Given nums = [0,1,2,2,3,0,4,2], val = 2,
Your function should return length = 5, with the first five elements of nums containing 0, 1, 3, 0, and 4.
Note that the order of those five elements can be arbitrary.
It doesn't matter what values are set beyond the returned length.

#### 解释下题目：
> 和之前的[026](https://www.jianshu.com/p/99c5693c290a)一样，只是这次给一个指定的数来删除


## 1. 处理数组
#### 实际耗时：8ms
```
public int removeElement(int[] nums, int val) {
        if (nums.length == 0) {
            return 0;
        }
        int count = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] != val) {
                nums[count] = nums[i];
                count++;
            }
        }
        return count;
    }
```
&emsp;&emsp;思路和之前的[026](https://www.jianshu.com/p/99c5693c290a)一样，只是换成了指定的数字而已。
###### 时间复杂度O(n) n为数组长度
###### 空间复杂度O(1) 
---------

