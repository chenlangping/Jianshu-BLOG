> Given an input string (`s`) and a pattern (`p`), implement regular expression matching with support for `'.'` and `'*'`.
`'.'` Matches any single character.
`'*'` Matches zero or more of the preceding element.
The matching should cover the **entire** input string (not partial).
#### Example：
> Input:
s = "aa"
p = "a"
Output: false
Explanation: "a" does not match the entire string "aa".

> Input:
s = "aa"
p = "a*"
Output: true
Explanation: '*' means zero or more of the precedeng element, 'a'. Therefore, by repeating 'a' once, it becomes "aa".

> Input:
s = "ab"
p = ".*"
Output: true
Explanation: ".*" means "zero or more (*) of any character (.)".

> Input:
s = "aab"
p = "c*a*b"
Output: true
Explanation: c can be repeated 0 times, a can be repeated 1 time. Therefore it matches "aab".

> Input:
s = "mississippi"
p = "mis*is*p*."
Output: false
#### Note：
> `s` could be empty and contains only lowercase letters `a-z`.
p could be empty and contains only lowercase letters `a-z`, and characters like `.` or `*`.

#### 解释下题目：
> 根据给定的规则字符串匹配


## 1. 动态规划
#### 实际耗时：22ms
```
public boolean isMatch(String s, String p) {
        if (s == null || p == null) {
            return false;
        }
        boolean[][] dp = new boolean[s.length() + 1][p.length() + 1];
        dp[0][0] = true;

        //其实有隐含条件：p的第一个肯定不能是*
        for (int i = 1; i < p.length(); i++) {
            if (p.charAt(i) == '*' && dp[0][i - 1]) {
                dp[0][i + 1] = true;
            }
        }

        for (int i = 0; i < s.length(); i++) {
            for (int j = 0; j < p.length(); j++) {
                //如果是"." 说明可以匹配任何，直接往下面去即可
                if (p.charAt(j) == '.') {
                    dp[i + 1][j + 1] = dp[i][j];
                    continue;
                }
                //两者完全一致，说明可以匹配
                if (p.charAt(j) == s.charAt(i)) {
                    dp[i + 1][j + 1] = dp[i][j];
                    continue;
                }
                //最难的一个，如果是"*"，说明可以匹配一个或者0个
                if (p.charAt(j) == '*') {
                    //如果前一个不能匹配上，说明"*"只能是0个
                    if (p.charAt(j - 1) != s.charAt(i) && p.charAt(j - 1) != '.') {
                        dp[i + 1][j + 1] = dp[i + 1][j - 1];
                    } else {
                        //前一个匹配上，所以可以是多个
                        //三个分别是：            a*=a        a*=aa                a*=空
                        dp[i + 1][j + 1] = (dp[i + 1][j] || dp[i][j + 1] || dp[i + 1][j - 1]);
                    }
                }
            }
        }
        return dp[s.length()][p.length()];

    }
```
###### 踩过的坑："aa", "a*"
&emsp;&emsp;思路大部分在注释中，需要注意的是*的处理，它可以是0个，1个，2个（3个及以上可以在下一个循环中进行处理）
###### 时间复杂度O(nm)  n和m分别为两个字符串的长度
###### 空间复杂度O(nm)  n和m分别为两个字符串的长度
---------
