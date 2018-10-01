> Given a string, determine if it is a palindrome, considering only alphanumeric characters and ignoring cases.
#### Example：
> Input: "A man, a plan, a canal: Panama"
Output: true

> Input: "race a car"
Output: false
#### Note：
> For the purpose of this problem, we define empty string as valid palindrome.

#### 解释下题目：
> 给定一个字符串，只看其中的字母和数字，字母不区分大小写，判断是不是回文字符串


## 1. 老老实实做呗
#### 实际耗时：8ms
```
public boolean isPalindrome(String s) {
        char[] a = new char[s.length()];
        int len = s.length();
        if (0 == len) {
            return true;
        }
        int index = 0;
        int charLen = 0;
        //第一步，先去除除了字母数字以外的字符，并把所有的大写转成小写
        while (index < len) {
            if (isLetter(s.charAt(index)) || isNumber(s.charAt(index))) {
                a[charLen++] = Character.toLowerCase(s.charAt(index));
            }
            index++;
        }
        //第二步，回文判断
        for (int i = 0; i < charLen / 2; i++) {
            if (a[i] != a[charLen - i - 1]) {
                return false;
            }
        }
        return true;
    }

    /**
     * 判断是否是字母
     *
     * @param a
     * @return true = 是字母
     */
    public static boolean isLetter(char a) {
        if (0 <= a - 'a' && 26 > a - 'a') {
            return true;
        }
        if (0 <= a - 'A' && 26 > a - 'A') {
            return true;
        }
        return false;
    }

    /**
     * 判断所给字符是否是0-9这十个数字
     *
     * @param num 所给字符
     * @return true = 是数字 false = 不是数字
     */
    public static boolean isNumber(char num) {
        if ((0 <= num - '0') && (10 > num - '0')) {
            return true;
        }
        return false;
    }
```
###### 踩过的坑："0P" 一开始我以为只保留字母......
&emsp;&emsp;思路很简单，剔除除了字母和数字以外的别的，生成一个新的数组，然后用回文数的方法进行判断即可。
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------
