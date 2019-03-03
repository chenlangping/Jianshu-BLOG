> Given two binary strings, return their sum (also a binary string).
The input strings are both **non-empty** and contains only characters `1` or `0`.
#### Example：
> Input: a = "11", b = "1"
Output: "100"

> Input: a = "1010", b = "1011"
Output: "10101"

#### 解释下题目：
> 给定两个指定的二进制字符串，输出它们的和


## 1. 【错误的方法】进制转换
#### 实际耗时：无限大ms
```
public String addBinary(String a, String b) {
        String result = "";
        long numa = stringToNumber(a);
        long numb = stringToNumber(b);
        long sum = numa + numb;
        result = numberToBinary(sum);
        return result;
    }

     /**
     * 将二进制的字符串变成十进制的数字
     *
     * @param s 二进制的字符串
     * @return 转换好的十进制数字
     */
    public long stringToNumber(String s) {
        long result = 0;
        int len = s.length() - 1;
        long n = 1;
        while (len >= 0) {
            if ('1' == s.charAt(len)) {
                result += n;
            }
            n *= 2;
            len--;
        }
        return result;
    }

    /**
     * 将十进制的数字转变成二进制的字符串
     *
     * @param number 要转变的数字
     * @return 转换好的字符串
     */
    public String numberToBinary(long number) {
        if (0 == number) {
            return "0";
        }
        StringBuilder sb = new StringBuilder();
        long n;
        while (number > 0) {
            n = number % 2;
            sb.append(n);
            number /= 2;
        }
        return sb.reverse().toString();
    }
```
###### 踩过的坑：这个它可以超级长........长到超过int，超过 long .......
&emsp;&emsp;思路就是把两个二进制的数字变成十进制，然后相加，最后再转换回十进制。
###### 时间复杂度O(n)   n为字符串长度
###### 空间复杂度O(1)
---------
## 2. 字符串处理
#### 实际耗时：3ms
```
public String addBinary2(String a, String b) {
        StringBuilder sb = new StringBuilder();
        int i = a.length() - 1;
        int j = b.length() - 1;
        //flag用来记录进位
        int flag = 0;
        while (i >= 0 || j >= 0) {
            int sum = flag;
            if (i >= 0) {
                sum += a.charAt(i) - '0';
                i--;
            }
            if (j >= 0) {
                sum += b.charAt(j) - '0';
                j--;
            }
            sb.append(sum % 2);
            flag = sum >> 1;
        }
        //如果最后还有进位位
        if (flag != 0) {
            sb.append(flag);
        }
        return sb.reverse().toString();
    }
```
&emsp;&emsp;思路跟之前的十进制加法一模一样。我到现在都没搞懂为什么我会想到去进制转换.......
###### 时间复杂度O(n)   n为字符串长度
###### 空间复杂度O(1)
---------
