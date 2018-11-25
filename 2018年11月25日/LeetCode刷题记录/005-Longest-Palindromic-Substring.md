> Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000.
#### Example：
> Input: "babad"
Output: "bab"

> Input: "cbbd"
Output: "bb"
#### Note：
> 有些题目的最大回文子串不一定是唯一的

#### 解释下题目：
> 找出最大回文子串


## 1. 老老实实找
#### 实际耗时：978ms
```
public String longestPalindrome(String s) {
        if (0 == s.length()) {
            return "";
        }
        if (1 == s.length()) {
            return s;
        }
        String result = String.valueOf(s.charAt(0));
        int maxLength = 1;
        for (int i = 0; i < s.length(); i++) {
            int j = s.length() - 1;
            while (j > i) {
                if (s.charAt(i) == s.charAt(j)) {
                    String sub = s.substring(i, j + 1);
                    if (isPalindrome(sub) && sub.length() > maxLength) {
                        maxLength = sub.length();
                        result = sub;
                        break;
                    }
                }
                j--;
            }
        }
        return result;
    }

    /**
     * 给定字符串，判定其是不是回文字符串
     *
     * @param s 需要判断的字符串
     * @return true = 是回文
     */
    public static boolean isPalindrome(String s) {
        if (0 == s.length()) {
            return true;
        }
        for (int i = 0; i < s.length() / 2; i++) {
            if (s.charAt(i) != s.charAt(s.length() - i - 1)) {
                return false;
            }
        }
        return true;
    }
```
###### 踩过的坑："aaabaaaa"  
&emsp;&emsp;思路就是首先编写一个函数，来确定所给的字符串是不是回文。然后就通过暴力遍历去找呗，因为回文的标志肯定是第一个和最后一个相等（这里其实可以稍微优化下，但是意义不大），根据这个来找就可以了。但是时间复杂度挺高的。
###### 时间复杂度O(n^3)
###### 空间复杂度O(1)
---------
## 2. 在每个位置开始寻找回文子串，[原作者](https://leetcode.com/problems/longest-palindromic-substring/discuss/2928/Very-simple-clean-java-solution)
#### 实际耗时：14ms
```
private int lo, maxLen;

    public String longestPalindrome2(String s) {
        int len = s.length();
        if (len < 2) {
            return s;
        }
        for (int i = 0; i < len - 1; i++) {
            //考虑回文是奇数的情况
            extendPalindrome(s, i, i);
            //考虑回文是偶数的情况
            extendPalindrome(s, i, i + 1); //assume even length.
        }
        return s.substring(lo, lo + maxLen);
    }

    private void extendPalindrome(String s, int j, int k) {
        while (j >= 0 && k < s.length() && s.charAt(j) == s.charAt(k)) {
            j--;
            k++;
        }
        if (maxLen < k - j - 1) {
            lo = j + 1;
            maxLen = k - j - 1;
        }
    }
```
&emsp;&emsp;思路就是，我一共遍历这个string的所有位子，以每个位置为中心，尽可能地拓宽这个回文数组。也可以这么理解，最大的回文子串必定有一个中心，如果这个回文子串的长度是奇数，那么就是中间那个数，否则就是中间的那两个数，那么我们只需要找到这个数（这两个数），然后不断拓宽它们，就是最大回文子串啦。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
