> Write a program to solve a Sudoku puzzle by filling the empty cells.


#### Example：
> ![数独](http://upload-images.jianshu.io/upload_images/13050335-d92f214a4ae9919f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![数独的解](http://upload-images.jianshu.io/upload_images/13050335-060146165e2baabc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 解释下题目：
> 给定一个9x9的矩阵，空格用点代替，来求解。数组是指行，列与九宫格都是单独的


## 1. backtracking
#### 实际耗时：12ms
```
public void solveSudoku(char[][] board) {
        solve(board);
        Matrix.print(board);
    }

    private boolean solve(char[][] board) {
        for (int i = 0; i < 9; i++) {
            for (int j = 0; j < 9; j++) {
                if (board[i][j] == '.') {
                    for (char k = '1'; k <= '9'; k++) {
                        if (check(board, i, j, k)) {
                            board[i][j] = k;
                            if (solve(board)) {
                                return true;
                            } else {
                                board[i][j] = '.';
                            }
                        }
                    }
                    return false;
                }
            }
        }
        return true;
    }

    private boolean check(char[][] board, int row, int col, char k) {
        //行的确认
        for (int j = 0; j < 9; j++) {
            if (board[row][j] == k) {
                return false;
            }
        }

        //列的确认
        for (int i = 0; i < 9; i++) {
            if (board[i][col] == k) {
                return false;
            }
        }

        //九宫格的确认
        for (int i = 0; i < 9; i++) {
            if (board[3 * (row / 3) + i / 3][3 * (col / 3) + i % 3] == k) {
                return false;
            }
        }

        return true;
    }

```
&emsp;&emsp;思路就是我先试试看能不能填呗，能填入就继续做下去，否则就跳回上一步。
###### 时间复杂度O(9^n)  n为点的个数，即需要填入的数字个数
###### 空间复杂度O(1)
---------
