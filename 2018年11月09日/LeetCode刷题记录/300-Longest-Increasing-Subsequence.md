> Given an unsorted array of integers, find the length of longest increasing subsequence.
#### Example：
> Input: [10,9,2,5,3,7,101,18]
Output: 4 
Explanation: The longest increasing subsequence is [2,3,7,101], therefore the length is 4. 
#### Note：
> There may be more than one LIS combination, it is only necessary for you to return the length.
Your algorithm should run in O(n^2) complexity.

#### 解释下题目：
> 找到数组中最长的递增序列


## 1. 动态规划+二分查找
#### 实际耗时：1ms
```
public int lengthOfLIS(int[] nums) {
        int[] dp = new int[nums.length];
        int length = 0;
        for (int num : nums) {
            int i = 0, j = length;
            while (i != j) {
                int m = (i + j) / 2;
                if (dp[m] < num) {
                    i = m + 1;
                } else {
                    j = m;
                }
            }
            dp[i] = num;
            if (i == length) {
                //如果目前的这个数是当前的递增数列最后一个
                length++;
            }
        }
        return length;
    }
```
&emsp;&emsp;思路就是维护一个最长的递增数列。
&emsp;&emsp;假设有个数组`{10, 9, 2, 5, 3, 7, 101, 18, 9, 13, 1, 2, 3, 4, 5, 6, 7}`，用肉眼看答案勉强能看出来是7，因为最后的1234567是递增序列，前面最长的`2,3,7,9,13`，用这个算法得到的每一轮的结果如下所示：
`10 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 `
`9 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 `
`2 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 `
`2 5 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 `
`2 3 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 `
`2 3 7 0 0 0 0 0 0 0 0 0 0 0 0 0 0 `
`2 3 7 101 0 0 0 0 0 0 0 0 0 0 0 0 0 `
`2 3 7 18 0 0 0 0 0 0 0 0 0 0 0 0 0 `
`2 3 7 9 0 0 0 0 0 0 0 0 0 0 0 0 0 `
`2 3 7 9 13 0 0 0 0 0 0 0 0 0 0 0 0 `
1 3 7 9 13 0 0 0 0 0 0 0 0 0 0 0 0 
`1 2 7 9 13 0 0 0 0 0 0 0 0 0 0 0 0 `
`1 2 3 9 13 0 0 0 0 0 0 0 0 0 0 0 0 `
`1 2 3 4 13 0 0 0 0 0 0 0 0 0 0 0 0 `
`1 2 3 4 5 0 0 0 0 0 0 0 0 0 0 0 0 `
`1 2 3 4 5 6 0 0 0 0 0 0 0 0 0 0 0 `
`1 2 3 4 5 6 7 0 0 0 0 0 0 0 0 0 0 `
&emsp;&emsp;注意没有变色的那一行，好好理解就能理解代码。还有这是覆盖操作的。
###### 时间复杂度O(nlogn)
###### 空间复杂度O(n)
---------
