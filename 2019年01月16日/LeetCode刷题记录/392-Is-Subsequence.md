> Given a string `s` and a string `t`, check if `s` is subsequence of `t`.
You may assume that there is only lower case English letters in both `s` and `t`. `t` is potentially a very long (length ~= 500,000) string, and `s` is a short string (<=100).
A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ace" is a subsequence of "abcde" while "aec" is not).
#### Example：
> s = "abc", t = "ahbgdc" true
> s = "axc", t = "ahbgdc" false

#### 解释下题目：
> 判断s是不是t的子串


## 1. 贪心算法
#### 实际耗时：37ms
```
public boolean isSubsequence(String s, String t) {
        if (s == null || t == null) {
            return false;
        } else if (s.length() == 0) {
            return true;
        }
        int curIndexOfS = 0;
        int curIndexOfT = 0;
        while (curIndexOfT < t.length()) {
            if (s.charAt(curIndexOfS) == t.charAt(curIndexOfT)) {
                curIndexOfS++;
                curIndexOfT++;
                if (curIndexOfS == s.length()) {
                    return true;
                }
                continue;
            }
            curIndexOfT++;
        }
        return false;
    }
```
###### 踩过的坑：s为""的时候，对于任何情况都是true
&emsp;&emsp;思路很简单，贪心算法一个一个匹配喽
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
