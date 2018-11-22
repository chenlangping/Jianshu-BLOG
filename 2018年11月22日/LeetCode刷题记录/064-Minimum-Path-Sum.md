> Given a *m* x *n* grid filled with non-negative numbers, find a path from top left to bottom right which *minimizes* the sum of all numbers along its path.
#### Example：
> Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.

#### Note：
> You can only move either down or right at any point in time.

#### 解释下题目：
> 从左上角走到右下角，找到数值最小的那一条路，你只能往下走或者往右走。


## 1. 动态规划法
#### 实际耗时：6ms
```
public int minPathSum(int[][] grid) {
        if (grid.length <= 0) {
            return 0;
        }
        int[][] result = new int[grid.length][grid[0].length];
        result[0][0] = grid[0][0];
        //最左边的一列
        for (int i = 1; i < grid.length; i++) {
            result[i][0] = grid[i][0] + result[i - 1][0];
        }

        //最上面的一行
        for (int j = 1; j < grid[0].length; j++) {
            result[0][j] = grid[0][j] + result[0][j - 1];
        }

        for (int i = 1; i < grid.length; i++) {
            for (int j = 1; j < grid[0].length; j++) {
                result[i][j] = grid[i][j] + Math.min(result[i - 1][j], result[i][j - 1]);
            }
        }
        return result[grid.length - 1][grid[0].length - 1];
    }
```
&emsp;&emsp;思路：就是在之前的基础上加一个判断，到底是从上面走过来开销小呢，还是从左边走过来开销小呢。
###### 时间复杂度O(nm)
###### 空间复杂度O(nm)
---------
