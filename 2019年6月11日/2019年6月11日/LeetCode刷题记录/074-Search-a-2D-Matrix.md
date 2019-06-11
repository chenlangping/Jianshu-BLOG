
Write an efficient algorithm that searches for a value in an *m* x *n* matrix. This matrix has the following properties:
* Integers in each row are sorted from left to right.
* The first integer of each row is greater than the last integer of the previous row. 
#### Example：
> Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 3
Output: true

> Input:
matrix = [
  [1,   3,  5,  7],
  [10, 11, 16, 20],
  [23, 30, 34, 50]
]
target = 13
Output: false

#### 解释下题目：
> 因为二维数组其实就是一维数组，所以其实就是二分法查找


## 1. 二分法查找
#### 实际耗时：6ms
```
public boolean searchMatrix(int[][] matrix, int target) {
        int row = matrix.length;
        if (0 == row) {
            return false;
        }
        int column = matrix[0].length;
        int small = 0;
        int big = row * column - 1;
        int mid;
        int curRow;
        int curCol;
        while (big >= small) {
            mid = (big + small) / 2;
            curCol = mid % column;
            curRow = (mid - curCol) / column;
            if (target == matrix[curRow][curCol]) {
                return true;
            } else if (target > matrix[curRow][curCol]) {
                small = mid + 1;
            } else {
                big = mid - 1;
            }
        }

        return false;
    }
```
###### 踩过的坑：空的二维数组
&emsp;&emsp;思路就是把二维数组的两个维度按照`curRow * column + curCol = mid`这个方法转换一下就行了
###### 时间复杂度O(log(n))
###### 空间复杂度O(1)
---------


