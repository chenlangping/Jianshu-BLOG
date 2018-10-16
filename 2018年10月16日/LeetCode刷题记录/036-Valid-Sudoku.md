> Determine if a 9x9 Sudoku board is valid. Only the filled cells need to be validated **according to the following rules**:
Each row must contain the digits `1-9` without repetition.
Each column must contain the digits `1-9` without repetition.
Each of the 9 `3x3` sub-boxes of the grid must contain the digits `1-9` without repetition.
![示意图](http://upload-images.jianshu.io/upload_images/13050335-6ace17299a23be50.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
The Sudoku board could be partially filled, where empty cells are filled with the character `'.'`.
#### Example：
> Input:
[
  ["5","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: true

> Input:
[
  ["8","3",".",".","7",".",".",".","."],
  ["6",".",".","1","9","5",".",".","."],
  [".","9","8",".",".",".",".","6","."],
  ["8",".",".",".","6",".",".",".","3"],
  ["4",".",".","8",".","3",".",".","1"],
  ["7",".",".",".","2",".",".",".","6"],
  [".","6",".",".",".",".","2","8","."],
  [".",".",".","4","1","9",".",".","5"],
  [".",".",".",".","8",".",".","7","9"]
]
Output: false
Explanation: Same as Example 1, except with the 5 in the top left corner being 
    modified to 8. Since there are two 8's in the top left 3x3 sub-box, it is invalid.
#### Note：
> A Sudoku board (partially filled) could be valid but is not necessarily solvable.
Only the filled cells need to be validated according to the mentioned rules.
The given board contain only digits `1-9` and the character `'.'`.
The given board size is always `9x9`.

#### 解释下题目：
> 按照它所给的三个条件一一去确认即可。


## 1. 按行按列按块进行查找
#### 实际耗时：35ms
```
 public boolean isValidSudoku(char[][] board) {
        //按列检查
        for (int i = 0; i < 9; i++) {
            List<Character> list = new ArrayList<>();
            for (int j = 0; j < 9; j++) {
                if (list.contains(board[i][j])) {
                    return false;
                } else if (isNumber(board[i][j])) {
                    list.add(board[i][j]);
                }
            }
        }

        //按行检查
        for (int j = 0; j < 9; j++) {
            List<Character> list = new ArrayList<>();
            for (int i = 0; i < 9; i++) {
                if (list.contains(board[i][j])) {
                    return false;
                } else if (isNumber(board[i][j])) {
                    list.add(board[i][j]);
                }
            }
        }

        for (int k = 0; k < 3; k++) {
            List<Character> list = new ArrayList<>();
            for (int i = 3 * k; i < 3 * k + 3; i++) {
                for (int j = 0; j < 3; j++) {
                    if (list.contains(board[i][j])) {
                        return false;
                    } else if (isNumber(board[i][j])) {
                        list.add(board[i][j]);
                    }
                }
            }
            list.clear();
            for (int i = 3 * k; i < 3 * k + 3; i++) {
                for (int j = 3; j < 6; j++) {
                    if (list.contains(board[i][j])) {
                        return false;
                    } else if (isNumber(board[i][j])) {
                        list.add(board[i][j]);
                    }
                }
            }
            list.clear();
            for (int i = 3 * k; i < 3 * k + 3; i++) {
                for (int j = 6; j < 9; j++) {
                    if (list.contains(board[i][j])) {
                        return false;
                    } else if (isNumber(board[i][j])) {
                        list.add(board[i][j]);
                    }
                }
            }
        }


        return true;
    }

    public static boolean isNumber(char a) {
        if (a - '0' >= 0 && a - '0' <= 9) {
            return true;
        }
        return false;
    }
```
&emsp;&emsp;主要就是利用了list的contain来判断有没有重复
###### 时间复杂度O(3*n^2)
###### 空间复杂度O(1)
---------
## 2. 少写点代码呗
#### 实际耗时：30ms
```
public boolean isValidSudoku2(char[][] board) {
        for (int i = 0; i < 9; i++) {
            HashSet<Character> rows = new HashSet<Character>();
            HashSet<Character> columns = new HashSet<Character>();
            HashSet<Character> cube = new HashSet<Character>();
            for (int j = 0; j < 9; j++) {
                if (board[i][j] != '.' && !rows.add(board[i][j]))
                    return false;
                if (board[j][i] != '.' && !columns.add(board[j][i]))
                    return false;
                int RowIndex = 3 * (i / 3);
                int ColIndex = 3 * (i % 3);
                if (board[RowIndex + j / 3][ColIndex + j % 3] != '.' && !cube.add(board[RowIndex + j / 3][ColIndex + j % 3]))
                    return false;
            }
        }
        return true;
    }
```
&emsp;&emsp;他巧妙的地方在于对于`3x3`的块的处理。
###### 时间复杂度O(3*n^2)
###### 空间复杂度O(1)
---------

