> A robot is located at the top-left corner of a *m* x *n* grid (marked 'Start' in the diagram below).
The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid (marked 'Finish' in the diagram below).
Now consider if some obstacles are added to the grids. How many unique paths would there be?
#### Example：
> Input:
[
  [0,0,0],
  [0,1,0],
  [0,0,0]
]
Output: 2
Explanation:
There is one obstacle in the middle of the 3x3 grid above.
There are two ways to reach the bottom-right corner:
> 1. Right -> Right -> Down -> Down
> 2. Down -> Down -> Right -> Right
#### Note：
>  *m* and *n* will be at most 100.

#### 解释下题目：
> 在[062](https://www.jianshu.com/p/0ac395d13075)的基础上加了点障碍物，仅此而已


## 1. 还是动态规划法
#### 实际耗时：1ms
```
public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        //特殊情况判断
        if (0 == obstacleGrid.length) {
            return 0;
        }
        //赋予初值，需要注意如果遇到障碍，后面的就不用赋初值了(因为根本到不了)
        int[][] result = new int[obstacleGrid.length][obstacleGrid[0].length];
        for (int i = 0; i < obstacleGrid.length; i++) {
            if (obstacleGrid[i][0] == 1) {
                break;
            } else {
                result[i][0] = 1;
            }
        }

        for (int j = 0; j < obstacleGrid[0].length; j++) {
            if (obstacleGrid[0][j] == 1) {
                break;
            } else {
                result[0][j] = 1;
            }
        }

        //“递归”求解
        for (int i = 1; i < obstacleGrid.length; i++) {
            for (int j = 1; j < obstacleGrid[0].length; j++) {
                if (1 == obstacleGrid[i][j]) {
                    result[i][j] = 0;
                } else {
                    result[i][j] = result[i - 1][j] + result[i][j - 1];
                }
            }
        }
        return result[obstacleGrid.length - 1][obstacleGrid[0].length - 1];
    }
```
&emsp;&emsp;思路：跟[062](https://www.jianshu.com/p/0ac395d13075)几乎一模一样，只是加上了障碍物的判断，如果有障碍物说明走不通，那么在赋予初值的时候记得对障碍物下面的和障碍物右边的数进行处理，然后在进行动态规划的时候把数组的这个值置成0，方便之后的处理。
###### 时间复杂度O(nm)，n和m是二维数组的长宽
###### 空间复杂度O(1)
---------

