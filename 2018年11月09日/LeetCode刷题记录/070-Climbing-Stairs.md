> You are climbing a stair case. It takes *n* steps to reach to the top.
Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?
#### Example：
> Input: 2
Output: 2
Explanation: There are two ways to climb to the top.
1 step + 1 step
2 steps

> Input: 3
Output: 3
Explanation: There are three ways to climb to the top.
1 step + 1 step + 1 step
1 step + 2 steps
2 steps + 1 step
#### Note：
> Given *n* will be a positive integer.

#### 解释下题目：
> 走楼梯的问题


## 1. 利用斐波那契数列的解法
#### 实际耗时：超时
```
public int climbStairs(int n) {
        if (0 == n) {
            return 0;
        }
        if (1 == n) {
            return 1;
        }
        if (2 == n) {
            return 2;
        }
        return climbStairs(n - 1) + climbStairs(n - 2);
    }
```
&emsp;&emsp;思路很简单，就是太费时间了。
###### 时间复杂度 [图片上传失败...(image-f33589-1537284481624)]
###### 空间复杂度O(1)
---------
## 2. 动态规划
#### 实际耗时：3ms
```
public int climbStairs2(int n) {
        if (1 == n) {
            return 1;
        }
        int[] result = new int[n + 1];
        result[1] = 1;
        result[2] = 2;
        for (int i = 3; i <= n; i++) {
            result[i] = result[i - 1] + result[i - 2];
        }
        return result[n];
    }
```
&emsp;&emsp;思路其实和斐波那契数列的思路是一样的，只不过用了动态规划把数据存起来罢了。
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------
