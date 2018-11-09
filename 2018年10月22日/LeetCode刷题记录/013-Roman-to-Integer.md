> Roman numerals are represented by seven different symbols: I, V, X, L, C, D and M.

|Symbol|Value|
|:--|:--|
|I |1|
|V      |       5|
|X       |      10|
|L        |     50|
|C       |      100|
|D       |      500|
|M        |     1000|
For example, two is written as `II `in Roman numeral, just two one's added together. Twelve is written as, `XII`, which is simply `X + II`. The number twenty-seven is written as `XXVII`, which is `XX + V + II`.

Roman numerals are usually written largest to smallest from left to right. However, the numeral for four is not `IIII`. Instead, the number four is written as `IV`. Because the one is before the five we subtract it making four. The same principle applies to the number nine, which is written as `IX`. There are six instances where subtraction is used:
* `I` can be placed before `V` (5) and `X` (10) to make 4 and 9. 
* `X` can be placed before `L` (50) and `C` (100) to make 40 and 90. 
* `C` can be placed before `D` (500) and `M` (1000) to make 400 and 900.

#### Example:
> Input: "III"     Output: 3
   Input: "IV"     Output: 4
   Input: "IX"     Output: 9
   Input: "LVIII"  Output: 58  Explanation: C = 100, L = 50, XXX = 30 and III = 3.
   Input: "MCMXCIV"   Output: 1994   Explanation: M = 1000, CM = 900, XC = 90 and IV = 4.

#### 翻译：
> 将用罗马字表达式翻译成阿拉伯数字即可。


## 1. 把特殊的值挑出来即可
#### 实际耗时：48ms
```
 public int romanToInt(String s) {
        int length = s.length();
        int sum = 0;
        for (int i = 0; i < length; i++) {
            char a = s.charAt(i);
            if (i < length - 1) {
                char b = s.charAt(i + 1);
                if (a == 'I' && b == 'V') {
                    sum += 4;
                    i++;
                    continue;
                } else if (a == 'I' && b == 'X') {
                    sum += 9;
                    i++;
                    continue;
                } else if (a == 'X' && b == 'L') {
                    sum += 40;
                    i++;
                    continue;
                } else if (a == 'X' && b == 'C') {
                    sum += 90;
                    i++;
                    continue;
                } else if (a == 'C' && b == 'D') {
                    sum += 400;
                    i++;
                    continue;
                } else if (a == 'C' && b == 'M') {
                    sum += 900;
                    i++;
                    continue;
                }
            }

            if (a == 'I') {
                sum += 1;
            }
            if (a == 'V') {
                sum += 5;
            }
            if (a == 'X') {
                sum += 10;
            }
            if (a == 'L') {
                sum += 50;
            }
            if (a == 'C') {
                sum += 100;
            }
            if (a == 'D') {
                sum += 500;
            }
            if (a == 'M') {
                sum += 1000;
            }
        }
        return sum;
    }
```
就是简单把那些`4` `9` `40` `90`这种数字挑出来，其余就加起来即可。
###### 时间复杂度O(n) (n为数字长度)
###### 空间复杂度O(1)
---------
## 2. 其他方法
#### 实际耗时：未知
```
暂时没有写
```
其实这种规定好了的规则下，还是用手工把所有情况都“遍历”一遍来的最快，其他无非就是把上面的规则翻译一下放进数组或者hashmap中，并不会加快算法效率。
###### 时间复杂度O(未知)
###### 空间复杂度O(未知)
