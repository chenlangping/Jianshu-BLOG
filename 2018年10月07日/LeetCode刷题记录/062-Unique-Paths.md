> A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).
The robot can only move either **down** or **right** at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
How many possible unique paths are there?

![机器人走路示意图](http://upload-images.jianshu.io/upload_images/13050335-da74ab4d8cfe45ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### Example：
> Input: m = 3, n = 2
Output: 3
Explanation:
From the top-left corner, there are a total of 3 ways to reach the bottom-right corner:
> 1. Right -> Right -> Down
> 2. Right -> Down -> Right
> 3. Down -> Right -> Right

> Input: m = 7, n = 3
Output: 28
#### Note：
> *m* and *n* will be at most `100`.

#### 解释下题目：
> 机器人每次只能往右走或者往下走，那么从图中的左上角走到右下角有多少种路径呢？


## 1. 动态规划
#### 实际耗时：0ms
```
public int uniquePaths(int m, int n) {
        if (m <= 0 || n <= 0) {
            return 0;
        }
        int[][] result = new int[m][n];
        //最左边的竖的一列
        for (int i = 0; i < m; i++) {
            result[i][0] = 1;
        }
        //最上面横的一列
        for (int j = 0; j < n; j++) {
            result[0][j] = 1;
        }
        //“递归”求解
        for (int i = 1; i < m; i++) {
            for (int j = 1; j < n; j++) {
                result[i][j] = result[i - 1][j] + result[i][j - 1];
            }
        }
        return result[m - 1][n - 1];
    }
```
&emsp;&emsp;思路：标准的动态规划，因为最左边的一列和最上面的一行只有一种解，如果我想要知道[i][j]有多少种走法，那只需要知道到这个格子的左边和这个格子的上面有多少种，相加即可。
###### 时间复杂度O(n*m)
###### 空间复杂度O(1)
---------

