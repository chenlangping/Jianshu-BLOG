> Given a 2D board and a word, find if the word exists in the grid.
The word can be constructed from letters of sequentially adjacent cell, where "adjacent" cells are those horizontally or vertically neighboring. The same letter cell may not be used more than once.
#### Example：
> board =
[
  ['A','B','C','E'],
  ['S','F','C','S'],
  ['A','D','E','E']
]
Given word = "ABCCED", return true.
Given word = "SEE", return true.
Given word = "ABCB", return false.


#### 解释下题目：
> 给定一个二维数组，然后给定一个String，判断这个String是否存在于这个二维数组中，相当于找一条折线，这个String可以用折线连起来在二维数组中找到。但是不能回头。


## 1. 递归查找
#### 实际耗时：15ms
```
public boolean exist(char[][] board, String word) {
        char[] newWord = word.toCharArray();
        for (int i = 0; i < board.length; i++) {
            for (int j = 0; j < board[i].length; j++) {
                if (board[i][j] != newWord[0]) {
                    continue;
                }
                if (recursion(board, newWord, i, j, 0)) {
                    return true;
                }
            }
        }
        return false;
    }

    public boolean recursion(char[][] board, char[] newWord, int x, int y, int index) {
        //这里这个判断条件反而是最难想到的，想象word=1的时候，它的四个方向都是错误的，但是因为这个判断可以使答案正确
        if (index == newWord.length) {
            return true;
        }
        //越界判断
        if (x < 0 || y < 0 || x >= board.length || y >= board[x].length) {
            return false;
        }
        //如果目前的位置和要求不一致
        if (board[x][y] != newWord[index]) {
            return false;
        }

        //这一步使borad[x][y]变成非字符
        char temp = board[x][y];
        board[x][y] = '我';
        //判断四个方向是否存在
        boolean result = recursion(board, newWord, x - 1, y, index + 1) ||
                recursion(board, newWord, x + 1, y, index + 1) ||
                recursion(board, newWord, x, y - 1, index + 1) ||
                recursion(board, newWord, x, y + 1, index + 1);
        //这一步使borad[x][y]变回来
        board[x][y] = temp;
        return result;
    }
```
###### 踩过的坑：最坑最坑的是我之前用了4个Boolean型，分别用来代替上下左右四个方向，然后把这四个Boolean型的值进行“或”操作，结果一直超时，这是因为其实并没有“短路”。
&emsp;&emsp;思路就是进去判断三个条件，**详见注释**，如果都符合那么就继续判断它的四个方向有没有符合要求的数字，以此重复。
###### 时间复杂度O(mn * 3^word.length())  mn分别为二维数组的长宽，这里是3而不是4的原因是因为并不能往回走。
###### 空间复杂度O(1)
---------


