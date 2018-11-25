> Implement `atoi` which converts a string to an integer.
The function first discards as many whitespace characters as necessary until the first non-whitespace character is found. Then, starting from this character, takes an optional initial plus or minus sign followed by as many numerical digits as possible, and interprets them as a numerical value.
The string can contain additional characters after those that form the integral number, which are ignored and have no effect on the behavior of this function.
If the first sequence of non-whitespace characters in str is not a valid integral number, or if no such sequence exists because either str is empty or it contains only whitespace characters, no conversion is performed.
If no valid conversion could be performed, a zero value is returned.
#### Example：
> Input: "42"
Output: 42

> Input: "   -42"
Output: -42
Explanation: The first non-whitespace character is '-', which is the minus sign.
             Then take as many numerical digits as possible, which gets 42.

> Input: "4193 with words"
Output: 4193
Explanation: Conversion stops at digit '3' as the next character is not a numerical digit.

> Input: "words and 987"
Output: 0
Explanation: The first non-whitespace character is 'w', which is not a numerical 
             digit or a +/- sign. Therefore no valid conversion could be performed.

> Input: "-91283472332"
Output: -2147483648
Explanation: The number "-91283472332" is out of the range of a 32-bit signed integer.
             Thefore INT_MIN (−231) is returned.

#### Note：
> Only the space character `' '` is considered as whitespace character.
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31,  2^31 − 1]. If the numerical value is out of the range of representable values, INT_MAX (2^31 − 1) or INT_MIN (−2^31) is returned.
#### 解释下题目：
> 给定一个字符串，转化数组


## 1. 老老实实挨个判断呗
#### 实际耗时：22ms
```
public int myAtoi(String str) {
        //result记录结果
        int result = 0;
        //numsofZero 记录字符串最开始的0的个数
        int numsofZero = 0;
        //i 用来记录位置
        int i = 0;
        //判断数字的正负，和有没有带+-符号
        boolean nagative = false;
        boolean hasSign = false;

        //如果长度是0,不用判断
        if (0 == str.length()) {
            return 0;
        }

        //去除开头的空格
        while (i < str.length()) {
            if (str.charAt(i) == ' ') {
                numsofZero++;
            } else {
                break;
            }
            i++;
        }

        //如果整个字符串全是空
        if (numsofZero == str.length()) {
            return 0;
        }

        //判断第一个是不是'+'或者'-'
        if ('+' == str.charAt(numsofZero)) {
            nagative = false;
            hasSign = true;
        } else if ('-' == str.charAt(numsofZero)) {
            nagative = true;
            hasSign = true;
        } else if (!isNumber(str.charAt(numsofZero))) {
            //不是数字且不是加减号
            return 0;
        }

        //此时确保整个字符串一定是去除开头的空格之后，由数字或者正负号开始的

        if (hasSign) {
            //如果有符号位，那么从下一位开始算
            i = numsofZero + 1;
        } else {
            //没有符号位，直接开始算
            i = numsofZero;
        }
        if (!nagative) {
            //是正数
            while (i < str.length() && isNumber(str.charAt(i))) {
                if (result == Integer.MAX_VALUE / 10 && charToInt(str.charAt(i)) > 7) {
                    return Integer.MAX_VALUE;
                } else if (result > Integer.MAX_VALUE / 10) {
                    return Integer.MAX_VALUE;
                }
                result = result * 10 + charToInt(str.charAt(i));
                i++;
            }
        } else {
            //是负数
            while (i < str.length() && isNumber(str.charAt(i))) {
                if (result == Integer.MIN_VALUE / 10 && charToInt(str.charAt(i)) > 8) {
                    return Integer.MIN_VALUE;
                } else if (result < Integer.MIN_VALUE / 10) {
                    return Integer.MIN_VALUE;
                }
                result = result * 10 - charToInt(str.charAt(i));
                i++;
            }
        }
        return result;
    }

    /**
     * char转int
     *
     * @param num 需要转换的字符
     * @return 字符对应的int，如果不是0-9之间的字符会回复0
     */
    public static int charToInt(char num) {
        if (isNumber(num)) {
            return num - '0';
        }
        return 0;
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
###### 踩过的坑："-2147483649"  "2147483800"  "2147483648"
&emsp;&emsp;第一个字符只可能是`空格`或者`+`或者 `-`或者 `数字`这四者之一。如果是空格，那么就不停往后走，走到末尾或者碰到`+`或者 `-`或者 `数字`这三者之一。如果是符号那就记录下这个数的正负，然后开始进行判断。用的是最正常的方法，唯一困扰我的是越界判断，正数其实还可以，负数需要特殊注意，我一开始是`charToInt(str.charAt(i)) < 8`是错的，因为负数其实是绝对值越大反而越小。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
