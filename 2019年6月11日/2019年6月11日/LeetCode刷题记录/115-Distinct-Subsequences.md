>Given a string **S** and a string **T**, count the number of distinct subsequences of **S** which equals **T**.

> A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "`ACE`" is a subsequence of "`ABCDE`" while "`AEC`" is not).
#### Example：
> Input: S = "rabbbit", T = "rabbit"
Output: 3
Explanation:
As shown below, there are 3 ways you can generate "rabbit" from S.
(The caret symbol ^ means the chosen letters)
rabbbit
^^^^ ^^
rabbbit
^^ ^^^^
rabbbit
^^^ ^^^


#### 解释下题目：
> 找出一个字符串里有多少个不同的另一个字符串。


## 1. 全匹配
#### 实际耗时：超时
```
public int numDistinct(String s, String t) {
        HashMap<Integer, ArrayList<Integer>> map = new HashMap<>();
        for (int i = 0; i < t.length(); i++) {
            ArrayList<Integer> tmp = new ArrayList<>();
            for (int j = 0; j < s.length(); j++) {

                if (t.charAt(i) == s.charAt(j)) {
                    tmp.add(j);
                }
            }
            if (tmp.size() == 0) {
                return 0;
            }
            map.put(i, tmp);
        }

        for (int i = 1; i < t.length(); i++) {
            map.put(0, helper(map.get(0), map.get(i)));
        }

        return map.get(0).size();


    }
```
&emsp;&emsp;思路很简单，就是去找出那个需要匹配的字符串的每个字符在另外一个字符里面所在位置，然后进行结合。由于涉及到排列的问题，所以时间复杂度很高
###### 时间复杂度O(n^m) 
###### 空间复杂度O(n^m)
---------

## 2. 动态规划
#### 实际耗时：5ms
```
public int numDistinct2(String s, String t) {
        int[][] res = new int[t.length() + 1][s.length() + 1];

        //1. 初始化
        for (int j = 0; j <= s.length(); j++) {
            res[0][j] = 1;
        }

        //2. 动态规划
        for (int i = 0; i < t.length(); i++) {
            for (int j = 0; j < s.length(); j++) {
                if (t.charAt(i) == s.charAt(j)) {
                    res[i + 1][j + 1] = res[i][j] + res[i + 1][j];
                } else {
                    res[i + 1][j + 1] = res[i + 1][j];
                }
            }
        }

        return res[t.length()][s.length()];
    }
```
&emsp;&emsp;思路很简单，而且我发现这种字符串匹配问题用这种二维数组+画图的解非常合适。
###### 时间复杂度O(mn)
###### 空间复杂度O(mn) mn分别是两个字符串的长度。
