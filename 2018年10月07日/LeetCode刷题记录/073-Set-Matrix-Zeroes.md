> Given a *m* x *n* matrix, if an element is 0, set its entire row and column to 0\. Do it [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm).

#### Example：
> Input: 
[
  [1,1,1],
  [1,0,1],
  [1,1,1]
]
Output: 
[
  [1,0,1],
  [0,0,0],
  [1,0,1]
]

> Input: 
[
  [0,1,2,0],
  [3,4,5,2],
  [1,3,1,5]
]
Output: 
[
  [0,0,0,0],
  [0,4,5,0],
  [0,3,1,0]
]
#### Follow up：
> * A straight forward solution using O(mn) space is probably a bad idea.额外开辟m*n的空间是很蠢的解决方法
> * A simple improvement uses O(m + n) space, but still not the best solution.开辟m+n的空间是还不错的方法，但是不是最好的
> * Could you devise a constant space solution?常数空间的方法有没有呢？

#### 解释下题目：
> 找到数组中数值是0的那项，把这个0所在的行和列都置成0


## 1. 开辟额外的空间呗
#### 实际耗时：1ms
```
public void setZeroes(int[][] matrix) {
        if (matrix.length == 0) return;
        int rows = matrix.length;//行数
        int columns = matrix[0].length;//列数
        boolean row[] = new boolean[rows];
        boolean column[] = new boolean[columns];
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < columns; j++) {
                if (0 == matrix[i][j]) {
                    //记录下ij的位置，但是先不改
                    row[i] = true;
                    column[j] = true;
                }
            }
        }

        //开始修改
        for (int i = 0; i < rows; i++) {
            if (row[i]) {
                setRowZero(matrix, i);
            }
        }
        for (int j = 0; j < columns; j++) {
            if (column[j]) {
                setColumnZero(matrix, j);
            }
        }
    }

    private void setColumnZero(int[][] matrix, int column) {
        for (int i = 0; i < matrix.length; i++) {
            matrix[i][column] = 0;
        }
    }

    private void setRowZero(int[][] matrix, int row) {
        for (int i = 0; i < matrix[0].length; i++) {
            matrix[row][i] = 0;
        }
    }
```
&emsp;&emsp;思路是另外开辟一个数组，暂时存放0的位置，接下来对数组进行处理。但是这个显然不是什么好方法。需要额外开辟一个m*n的空间。
###### 时间复杂度O(nm)
###### 空间复杂度O(nm)
---------
## 2. 只在原数组上修改
#### 实际耗时：2ms
```
private void setZeroes2(int[][] matrix) {
        if (matrix.length == 0) return;
        int row = matrix.length;//行数
        int column = matrix[0].length;//列数
        boolean rowSetZero = false;
        boolean columnSetZero = false;
        //从最左边的列从上向下开始判断
        for (int i = 0; i < row; i++) {
            if (0 == matrix[i][0]) {
                columnSetZero = true;
                break;
            }
        }
        //从第一行从左往右开始判断
        for (int j = 0; j < column; j++) {
            if (0 == matrix[0][j]) {
                rowSetZero = true;
                break;
            }
        }
        //开始判断
        for (int i = 1; i < row; i++) {
            for (int j = 1; j < column; j++) {
                if (0 == matrix[i][j]) {
                    matrix[0][j] = 0;
                    matrix[i][0] = 0;
                }
            }
        }


        //最左边的那一列
        for (int i = 1; i < row; i++) {
            if (0 == matrix[i][0]) {
                setRowZero(matrix, i);
            }
        }

        //第一行
        for (int j = 1; j < column; j++) {
            if (0 == matrix[0][j]) {
                setColumnZero(matrix, j);
            }
        }

        if (rowSetZero) {
            setRowZero(matrix, 0);
        }
        if (columnSetZero) {
            setColumnZero(matrix, 0);
        }
    }
```
&emsp;&emsp;思路就是数组的判断是从左到右，从上到下的，所以如果可以肯定的是位于目前数字的左上方的数字肯定是已经判断过了的，如果我目前的数字是0，那么我的此行的最左边和此列的最上方的数字肯定会被置成0，它们是几已经无所谓了。按照这个思路遍历所有的数字，如果x行y列的数字是0，那么最左边一列的x行被置成0，最上面一行的y列那个数字被置成0，最后根据这个来判断需不需要将整行/整列进行处理。需要注意的还需要两个额外的boolean来记录一开始最左边的一列和最上面的一行是不是带0(因为之后只要有0，那么最左边的一列和最上面的一行肯定会带0，所以需要记录)
###### 时间复杂度O(nm)
###### 空间复杂度O(1)
---------

