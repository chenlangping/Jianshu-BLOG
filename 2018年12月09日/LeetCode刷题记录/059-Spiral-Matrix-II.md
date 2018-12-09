> Given a positive integer *n*, generate a square matrix filled with elements from 1 to n*n in spiral order.
#### Example：
> Input: 3
Output:
[
 [ 1, 2, 3 ],
 [ 8, 9, 4 ],
 [ 7, 6, 5 ]
]

#### 解释下题目：
> 螺旋输出矩阵，且一定是正方形的


## 1. 老老实实输出呗
#### 实际耗时：2ms
```
public int[][] generateMatrix(int n) {
        if (n <= 0) {
            return new int[0][0];
        }
        int[][] matrix = new int[n][n];
        int rowBegin = 0;
        int rowEnd = n;
        int columnBegin = 0;
        int columnEnd = n;
        int cur = n * n;
        int num = 1;
        while (cur > 0) {
            //1.从左向右
            for (int j = rowBegin; j < rowEnd; j++) {
                matrix[columnBegin][j] = num;
                num++;
                cur--;
            }

            //2.从上到下
            for (int i = columnBegin + 1; i < columnEnd; i++) {
                matrix[i][rowEnd - 1] = num;
                num++;
                cur--;
            }

            //3.从右向左
            for (int j = rowEnd - 2; j >= rowBegin; j--) {
                matrix[columnEnd - 1][j] = num;
                num++;
                cur--;
            }

            //4.从下到上
            for (int i = columnEnd - 2; i > columnBegin; i--) {
                matrix[i][rowBegin] = num;
                num++;
                cur--;
            }
            rowBegin++;
            rowEnd--;
            columnBegin++;
            columnEnd--;

        }
        return matrix;
    }
```
&emsp;&emsp;思路：没什么好说的，老老实实输出呗
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------

