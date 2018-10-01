> Given an array with *n* objects colored red, white or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white and blue.
Here, we will use the integers 0, 1, and 2 to represent the color red, white, and blue respectively.

#### Example：
> Input: [2,0,2,1,1,0]
Output: [0,0,1,1,2,2]
#### Note：
> You are not suppose to use the library's sort function for this problem.

#### 解释下题目：
> 数组中只有三种元素，在这个前提下进行排序


## 1. 特殊优化法
#### 实际耗时：0ms
```
public void sortColors(int[] nums) {
        //把三种颜色记下来就行了
        int num1 = 0;
        int num2 = 0;
        int num3 = 0;
        for (int i = 0; i < nums.length; i++) {
            if (0 == nums[i]) {
                num1++;
            } else if (1 == nums[i]) {
                num2++;
            } else {
                num3++;
            }
        }

        //然后把数组搞一下就行了
        for (int i = 0; i < num1; i++) {
            nums[i] = 0;
        }

        for (int i = num1; i < num1 + num2; i++) {
            nums[i] = 1;
        }

        for (int i = num1 + num2; i < nums.length; i++) {
            nums[i] = 2;
        }
    }
```
&emsp;&emsp;思路：既然只有三个数字，那我每个数字出现了几次记下来，然后在对数组进行处理就行了。这里很容易思维定式，想到排序去。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------

