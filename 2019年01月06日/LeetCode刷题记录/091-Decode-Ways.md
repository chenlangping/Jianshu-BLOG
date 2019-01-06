> A message containing letters from `A-Z` is being encoded to numbers using the following mapping:
'A' -> 1
'B' -> 2
...
'Z' -> 26
Given a **non-empty** string containing only digits, determine the total number of ways to decode it.
#### Example：
> Input: "12"
Output: 2
Explanation: It could be decoded as "AB" (1 2) or "L" (12).

> Input: "226"
Output: 3
Explanation: It could be decoded as "BZ" (2 26), "VF" (22 6), or "BBF" (2 2 6).

#### 解释下题目：
> 给定一串数字组成的字符串，看看有多少种字母组合与之对应


## 1. 动态规划
#### 实际耗时：5ms
```
public int numDecodings(String s) {
        if (s == null || s.length() == 0) {
            return 0;
        }
        int n = s.length();
        int[] dp = new int[n + 1];
        dp[0] = 1;
        dp[1] = s.charAt(0) == '0' ? 0 : 1;
        for (int i = 2; i <= n; i++) {
            int num1 = Integer.valueOf(s.substring(i - 1, i));
            int num2 = Integer.valueOf(s.substring(i - 2, i));
            if (num1 >= 1 && num1 <= 9) {
                dp[i] += dp[i - 1];
            }
            if (num2 >= 10 && num2 <= 26) {
                dp[i] += dp[i - 2];
            }
        }
        return dp[n];
    }
```
&emsp;&emsp;思路：对于每一个位子，仅需要考虑两种情况：1.如果该位子的值是1-9，那么说明它可以单独作为一个编码；2.如果它和之前的一位组合在一起，可以是10-26之间（包含10和26），那么说明它还可以和之前的前一格进行组合。
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------
