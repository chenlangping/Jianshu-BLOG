> Given an array of non-negative integers, you are initially positioned at the first index of the array.
Each element in the array represents your maximum jump length at that position.
Determine if you are able to reach the last index.
#### Example：
> Input: [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.

> Input: [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.

#### 解释下题目：
> 给定一个非负的整数数组，每个数代表可以从这个位置开始**最多**可以走几步，然后不停看能否走到最后一个数。所以说其实最后一个数字是没有用的。


## 1. 从最后开始判断
#### 实际耗时：6ms
```
public boolean canJump(int[] nums) {
        boolean result = true;
        int i = nums.length - 1;

        //只有一个数字 不用判断
        if (nums.length == 1) {
            return true;
        }

        //第一个数字是0直接跳过，是不可能到达末尾的
        if (0 == nums[0]) {
            return false;
        }

        //从最后第二个数开始(最后一个数是几都无所谓)
        while (i >= 0) {
            i = canJump(nums, i);
            if (i == -1) {
                result = false;
                break;
            } else if (i == 0) {
                result = true;
                break;
            }
        }
        return result;
    }

    //判断end之前的位置能否到达下标为end的位置，如果能就返回该下标
    public int canJump(int[] nums, int end) {
        for (int i = end - 1; i >= 0; i--) {
            if (end - i <= nums[i]) {
                return i;
            }
        }
        return -1;
    }
```
###### 踩过的坑：
`{1,2}`
`{0,2,3}`
`{3,2,1,0,4}`
`{2, 3, 1, 1, 4}`
`{1, 2}`
`{0, 2, 3}`
`{0}`

&emsp;&emsp;基本上都在注释里了。从最后第二个判断，看它之前的数字哪个能到达它，如果能则返回那个数字所在的下标。如果找不到则返回-1。然后不断重复，直到i到达0，说明从`nums[0]`可以到达最后。**注意一开始已经排除了`nums[0]==0`这种情况了**。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------


