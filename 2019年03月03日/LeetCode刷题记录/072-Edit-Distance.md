> Given two words *word1* and *word2*, find the minimum number of operations required to convert *word1* to *word2*.
You have the following 3 operations permitted on a word:
1.Insert a character
2.Delete a character
3.Replace a character
#### Example：
> Input: word1 = "horse", word2 = "ros"
Output: 3
Explanation: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')

> Input: word1 = "intention", word2 = "execution"
Output: 5
Explanation: 
intention -> inention (remove 't')
inention -> enention (replace 'i' with 'e')
enention -> exention (replace 'n' with 'x')
exention -> exection (replace 'n' with 'c')
exection -> execution (insert 'u')

#### 解释下题目：
> 找到从一个字符串经过最少的变化次数


## 1. DP
#### 实际耗时：7ms
```
public int minDistance(String word1, String word2) {
        int m = word1.length();
        int n = word2.length();
        int matrix[][] = new int[m + 1][n + 1];

        //初始化
        for (int i = 0; i <= m; i++) {
            matrix[i][0] = i;
        }
        for (int i = 1; i <= n; i++) {
            matrix[0][i] = i;
        }

        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1.charAt(i - 1) == word2.charAt(j - 1)) {
                    matrix[i][j] = matrix[i - 1][j - 1];
                } else {
                    int replace = matrix[i - 1][j - 1];
                    int delete = matrix[i][j - 1];
                    int insert = matrix[i - 1][j];
                    if (replace <= delete && replace <= insert) {
                        matrix[i][j] = replace + 1;
                    } else if (delete <= replace && delete <= insert) {
                        matrix[i][j] = delete + 1;
                    } else {
                        matrix[i][j] = insert + 1;
                    }
                }
            }
        }

        return matrix[m][n];
    }
```
&emsp;&emsp;思路：上过卜东波老师的算法课之后就很简单了，因为课上讲了类似的题目。假设word1和word2分别在第i处和第j处断开，则假设word1的前i个字符需要经过n步变为word2的前j个字符串，则定义matrix[i][j] = n。所以如果word1[i] ==word2[j]的时候，直接照抄，而剩下的增删和修改对应三个位置即可。 
###### 时间复杂度O(mn)
###### 空间复杂度O(mn) 可以做到 O(n)
---------
