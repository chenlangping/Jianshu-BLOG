> Given *n*, how many structurally unique **BST's** (binary search trees) that store values *1 ... n*?
#### Example：
> Input: 3
Output: 5
![解释图](https://upload-images.jianshu.io/upload_images/13050335-a4155d086c064b11.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 解释下题目：
> 给定数字n，求出有几种二分搜索树与之对应


## 1. DP
#### 实际耗时：0ms
```
public int numTrees(int n) {
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = 1;
        for (int level = 2; level <= n; level++)
            for (int root = 1; root <= level; root++)
                dp[level] += dp[level - root] * dp[root - 1];
        return dp[n];
    }
```
&emsp;&emsp;思路没什么好说的，其实是个数学问题，理解为斐波那契数列就行了。
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------
