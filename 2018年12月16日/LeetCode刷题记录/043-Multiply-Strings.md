> Given two non-negative integers `num1` and `num2` represented as strings, return the product of `num1` and `num2`, also represented as a string.
#### Example：
> Input: num1 = "2", num2 = "3"
Output: "6"

> Input: num1 = "123", num2 = "456"
Output: "56088"
#### Note：
> The length of both `num1` and `num2` is < `110`.
Both `num1` and `num2` contain only digits `0-9`.
Both `num1` and `num2` do not contain any leading zero, except the number `0` itself.
You must not use any built-in BigInteger library or convert the inputs to integer directly.

#### 解释下题目：
> 两个大数相乘，记得不要使用内置的大数类


## 1. 老老实实做呗
#### 实际耗时：19ms
```
public String multiply(String num1, String num2) {
        StringBuilder stringBuilder = new StringBuilder();
        int m = num1.length();
        int n = num2.length();
        if (m == 0 || n == 0 || num1.equals("0") || num2.equals("0")) {
            return "0";
        }
        int[] res = new int[m + n];

        for (int i = m - 1; i >= 0; i--) {
            for (int j = n - 1; j >= 0; j--) {
                //两个对应位置的相乘
                int mul = (num1.charAt(i) - '0') * (num2.charAt(j) - '0');
                //和还要加上之前进位的(如果有的话)
                int sum = mul + res[i + j + 1];
                //填入
                res[i + j] += sum / 10;
                res[i + j + 1] = (sum) % 10;
            }
        }

        for (int i : res) {
            if (stringBuilder.length() == 0 && i == 0) {
                //去除开头的0
                continue;
            }
            stringBuilder.append(i);
        }

        return stringBuilder.toString();
    }
```
&emsp;&emsp;思路：首先需要明确的一点是，一个`m`位的数和一个`n`位的数相乘得到的最大解是`m+n`位的，其次是第`i`位和第`j`位的两个数字相乘影响的是第`i+j+i`位，如果有进位就会影响它前面一位，也就是`i+j`位。弄懂这些之后去处理上面的这些就很简单了。
###### 时间复杂度O(mn)   m和n分别是两个String的长度
###### 空间复杂度O(m+n)  m和n分别是两个String的长度
---------
