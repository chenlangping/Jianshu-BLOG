> The n-queens puzzle is the problem of placing n queens on an n×n chessboard such that no two queens attack each other.
![八皇后问题](http://upload-images.jianshu.io/upload_images/13050335-f0a59da3ccdc575f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
Given an integer `n`, return all distinct solutions to the n-queens puzzle.
Each solution contains a distinct board configuration of the n-queens' placement, where `'Q'` and `'.'`both indicate a queen and an empty space respectively.


#### Example：
> Input: 4
Output: [
 [".Q..",  // Solution 1
  "...Q",
  "Q...",
  "..Q."],
["..Q.",  // Solution 2
  "Q...",
  "...Q",
  ".Q.."]
]
Explanation: There exist two distinct solutions to the 4-queens puzzle as shown above.

#### 解释下题目：
> 标准的八皇后问题，只是这个需要打印出来而已


## 1. 递归调用
#### 实际耗时：7ms
```
public List<List<String>> solveNQueens(int n) {
        char[][] board = new char[n][n];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                board[i][j] = '.';
            }
        }
        List<List<String>> result = new ArrayList<List<String>>();

        if (n < 1) {
            return result;
        }
        recursive(board, 0, result);
        return result;
    }

    //递归函数
    private void recursive(char[][] board, int col, List<List<String>> result) {
        //如果已经遍历完了
        if (col == board.length) {
            result.add(transmit(board));
            return;
        }

        //从一行的最左边到最右边
        for (int i = 0; i < board.length; i++) {
            if (check(board, i, col)) {
                board[i][col] = 'Q';
                recursive(board, col + 1, result);
                board[i][col] = '.';
            }
        }
    }

    //确认能否放到xy处
    private boolean check(char[][] board, int x, int y) {
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < y; j++) {
                if (board[i][j] == 'Q' && (x + j == y + i || x + y == i + j || x == i))
                    return false;
            }
        }
        return true;
    }

    //转换结果
    private List<String> transmit(char[][] board) {
        List<String> result = new LinkedList<String>();
        for (int i = 0; i < board.length; i++) {
            String s = new String(board[i]);
            result.add(s);
        }
        return result;
    }
```
&emsp;&emsp;思路很简单，就是我每一行都从左往右边找，要放之前确认下能不能放，能放就放下去然后进到下一行，直到放完所有的棋子。
###### 时间复杂度O((n^3)n!) 
###### 空间复杂度O(1)
---------
