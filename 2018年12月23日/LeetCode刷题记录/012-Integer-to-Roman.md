> Roman numerals are represented by seven different symbols: `I`, `V`, `X`, `L`, `C`, `D` and `M`.

|Symbol |Value|  
|:--|:--|
|I       |      1|
|V      |       5
|X      |       10
|L       |      50
|C      |       100
|D       |      500
|M        |     1000

For example, two is written as `II` in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply` X` + `II`. The number twenty seven is written as `XXVII`, which is `XX` +`V`+ `II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:

`I` can be placed before `V` (5) and`X` (10) to make 4 and 9. 
`X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
`C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.
Given an integer, convert it to a roman numeral. Input is guaranteed to be within the range from 1 to 3999.
#### Example：
> Input: 3
Output: "III"

> Input: 4
Output: "IV"

>Input: 9
Output: "IX"

> Input: 58
Output: "LVIII"
Explanation: C = 100, L = 50, XXX = 30 and III = 3.

> Input: 1994
Output: "MCMXCIV"
Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.

#### 解释下题目：
> 就是给定一个数字，把它转成罗马数字，注意给定的数字是从1开始到3999


## 1. 分解的方法
#### 实际耗时：48ms
```
public String intToRoman(int num) {
        StringBuilder result = new StringBuilder();
        if (num < 10) {
            //只有一位数的时候，注意题设说了从1开始
            return oneNumberToRoman(num);
        } else if (num < 100) {
            //两位数的时候
            return twoNumberToRoman(num);
        } else if (num < 1000) {
            //三位数的时候
            return threeNumberToRoman(num);
        } else {
            //四位数的时候
            return fourNumberToRoman(num);
        }
    }

    public static String oneNumberToRoman(int num) {
        if (1 == num) {
            return "I";
        }
        if (2 == num) {
            return "II";
        }
        if (3 == num) {
            return "III";
        }
        if (4 == num) {
            return "IV";
        }
        if (5 == num) {
            return "V";
        }
        if (6 == num) {
            return "VI";
        }
        if (7 == num) {
            return "VII";
        }
        if (8 == num) {
            return "VIII";
        }
        if (9 == num) {
            return "IX";
        }
        return "";
    }

    public static String twoNumberToRoman(int num) {
        //x是十位数
        int x = num / 10;
        String result = "";
        if (1 == x) {
            result = "X";
        }
        if (2 == x) {
            result = "XX";
        }
        if (3 == x) {
            result = "XXX";
        }
        if (4 == x) {
            result = "XL";
        }
        if (5 == x) {
            result = "L";
        }
        if (6 == x) {
            result = "LX";
        }
        if (7 == x) {
            result = "LXX";
        }
        if (8 == x) {
            result = "LXXX";
        }
        if (9 == x) {
            result = "XC";
        }

        return result + oneNumberToRoman(num % 10);

    }

    public static String threeNumberToRoman(int num) {
        //c是百位数
        int c = num / 100;
        String result = "";
        if (1 == c) {
            result = "C";
        }
        if (2 == c) {
            result = "CC";
        }
        if (3 == c) {
            result = "CCC";
        }
        if (4 == c) {
            result = "CD";
        }
        if (5 == c) {
            result = "D";
        }
        if (6 == c) {
            result = "DC";
        }
        if (7 == c) {
            result = "DCC";
        }
        if (8 == c) {
            result = "DCCC";
        }
        if (9 == c) {
            result = "CM";
        }

        return result + twoNumberToRoman(num % 100);

    }

    public static String fourNumberToRoman(int num) {
        //m是千位数
        int m = num / 1000;
        String result = "";
        if (1 == m) {
            result = "M";
        }
        if (2 == m) {
            result = "MM";
        }
        if (3 == m) {
            result = "MMM";
        }
        return result + threeNumberToRoman(num % 1000);

    }
```
&emsp;&emsp;如果想知道1444这个数字是怎么转成罗马字的，我需要首先知道1000和444是怎么转成罗马字的；如果想知道444这个数字是怎么转成罗马字的，我需要知道400和44是怎么转成罗马字的，以此类推。
###### 时间复杂度O(1)
###### 空间复杂度O(1)
---------
## 2. 直接用数组来做，原解法点[这里](https://leetcode.com/problems/integer-to-roman/discuss/6274/Simple-Solution)
#### 实际耗时：53ms
```
public static String intToRoman2(int num) {
        String M[] = {"", "M", "MM", "MMM"};
        String C[] = {"", "C", "CC", "CCC", "CD", "D", "DC", "DCC", "DCCC", "CM"};
        String X[] = {"", "X", "XX", "XXX", "XL", "L", "LX", "LXX", "LXXX", "XC"};
        String I[] = {"", "I", "II", "III", "IV", "V", "VI", "VII", "VIII", "IX"};
        return M[num / 1000] + C[(num % 1000) / 100] + X[(num % 100) / 10] + I[num % 10];
    }
```
&emsp;&emsp;代码简单了很多，但是思路是一样的。
###### 时间复杂度O(1)
###### 空间复杂度O(1)
---------

