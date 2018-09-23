> You are given an n x n 2D matrix representing an image.
Rotate the image by 90 degrees (clockwise).
#### Example：
> Given input matrix = 
[
  [1,2,3],
  [4,5,6],
  [7,8,9]
],
rotate the input matrix in-place such that it becomes:
[
  [7,4,1],
  [8,5,2],
  [9,6,3]
]

> Given input matrix =
[
  [ 5, 1, 9,11],
  [ 2, 4, 8,10],
  [13, 3, 6, 7],
  [15,14,12,16]
], 
rotate the input matrix in-place such that it becomes:
[
  [15,13, 2, 5],
  [14, 3, 4, 1],
  [12, 6, 8, 9],
  [16, 7,10,11]
]
#### Note：
> You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.


#### 解释下题目：
>  将矩阵按照90度进行顺时针旋转，注意不要开一个新的二维数组来处理，必须在原数组上处理


## 1. 利用数学方法
#### 实际耗时：2ms
```
public void rotate(int[][] matrix) {
        int len = matrix.length;
        int temp = 0;

        //第一步，对角线交换
        for (int i = 0; i < len; i++) {
            for (int j = i + 1; j < len; j++) {
                temp = matrix[i][j];
                matrix[i][j] = matrix[j][i];
                matrix[j][i] = temp;
            }
        }

        //第二步，按照最中间的一列进行镜像交换
        for (int i = 0; i < len; i++) {
            for (int j = 0; j < len / 2; j++) {
                temp = matrix[i][j];
                matrix[i][j] = matrix[i][len - j - 1];
                matrix[i][len - j - 1] = temp;
            }
        }

}
```
&emsp;&emsp;先按照对角线进行交换，然后按照把第一列和最后一列进行交换。没什么技术含量。
###### 时间复杂度O(n^2)
###### 空间复杂度O(1)
---------
## 2. 按照实际的交换进行交换
#### 实际耗时：2ms
```
public void rotate2(int[][] matrix) {
        int n = matrix.length;
        for (int i = 0; i < n / 2; i++)
            for (int j = i; j < n - i - 1; j++) {
                int tmp = matrix[i][j];
                matrix[i][j] = matrix[n - j - 1][i];
                matrix[n - j - 1][i] = matrix[n - i - 1][n - j - 1];
                matrix[n - i - 1][n - j - 1] = matrix[j][n - i - 1];
                matrix[j][n - i - 1] = tmp;
            }
}
```
&emsp;&emsp;选取矩阵的四分之一，然后进行4次交换即可。
###### 时间复杂度O(n^2)
###### 空间复杂度O(1)
---------
