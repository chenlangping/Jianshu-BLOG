> Given an array of non-negative integers, you are initially positioned at the first index of the array.
Each element in the array represents your maximum jump length at that position.
Your goal is to reach the last index in the minimum number of jumps.
#### Example：
> Input: [2,3,1,1,4]
Output: 2
Explanation: The minimum number of jumps to reach the last index is 2. Jump 1 step from index 0 to 1, then 3 steps to the last index.
#### Note：
> You can assume that you can always reach the last index.

#### 解释下题目：
>  和之前的[055](https://www.jianshu.com/p/124ac389bd2b)差不多，只是这次它确保一定能到最后，要你找出跳跃次数最少的解


## 1. 从最后开始找
#### 实际耗时：156ms
```
public int jump(int[] nums) {
        int result = 0;
        int i = nums.length - 1;
        while (i > 0) {
            i = jumpMax(nums, i);
            result++;
        }
        return result;
    }

    //找到能够跳到的最远的数
    public int jumpMax(int[] nums, int end) {
        int min = -1;
        for (int i = end - 1; i >= 0; i--) {
            if (end - i <= nums[i]) {
                min = i;
            }
        }
        return min;
    }
```
&emsp;&emsp;思路和之前的[055](https://www.jianshu.com/p/124ac389bd2b)差不多，只是这次需要从最后位置开始，找到能够跳到**最远**的位置，因为从那个位置只需要跳一次就可以到最后。在那个位置和最后位置之间肯定不存在最优解。以此类推找到数组的头即可。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------

