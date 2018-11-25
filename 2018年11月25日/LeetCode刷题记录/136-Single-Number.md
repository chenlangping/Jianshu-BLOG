> Given a **non-empty** array of integers, every element appears twice except for one. Find that single one.

#### Example：
> Input: [2,2,1]
Output: 1

> Input: [4,1,2,1,2]
Output: 4
#### Note：
> Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

#### 解释下题目：
> 给定一个非空数组，其中每个数组都是有两个的，只有一个数字只有一个，找出那个数字


## 1. 抑或操作
#### 实际耗时：0ms
```
public int singleNumber(int[] nums) {
        //异或操作会留下最后的那一个数
        int result = nums[0];
        for (int i = 1; i < nums.length; i++) {
            result ^= nums[i];
        }
        return result;
    }
```
&emsp;&emsp;两个相同的数字异或是0
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
