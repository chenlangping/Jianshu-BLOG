> Given a string s, partition s such that every substring of the partition is a palindrome.
Return the minimum cuts needed for a palindrome partitioning of s.
#### Example：
> Input: "aab"
Output: 1
Explanation: The palindrome partitioning ["aa","b"] could be produced using 1 cut.

#### 解释下题目：
> 给定一个字符串，找出最少划几刀就可以让划分完的子集都是回文数。


## 1. 动态规划的方法
#### 实际耗时：17ms
```
public int minCut(String s) {
        char[] c = s.toCharArray();
        int n = c.length;
        int[] cut = new int[n];
        boolean[][] pal = new boolean[n][n];

        for(int i = 0; i < n; i++) {
            int min = i;
            for(int j = 0; j <= i; j++) {
                if(c[j] == c[i] && (j + 1 > i - 1 || pal[j + 1][i - 1])) {
                    pal[j][i] = true;
                    min = j == 0 ? 0 : Math.min(min, cut[j - 1] + 1);
                }
            }
            cut[i] = min;
        }
        return cut[n - 1];
    }
```

&emsp;&emsp;思路是这样的，如果c[j+1]到c[i-1]之间是回文数，而且又有c[j]==c[i]，那么相当于把回文数扩展了。pal[j][i]=true意味着c[j]到c[i]是回文数。
###### 时间复杂度O(n^2)
###### 空间复杂度O(n^2)
---------
