> The string `"PAYPALISHIRING"` is written in a zigzag pattern on a given number of rows like this: (you may want to display this pattern in a fixed font for better legibility)
![示意图](https://upload-images.jianshu.io/upload_images/13050335-8670307df7923a74.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
And then read line by line: `"PAHNAPLSIIGYIR"`
#### Example：
> Input: s = "PAYPALISHIRING", numRows = 3
Output: "PAHNAPLSIIGYIR"

> Input: s = "PAYPALISHIRING", numRows = 4
Output: "PINALSIGYAHRPI"

#### 解释下题目：
> 就是把所给的字符串按照N字母的顺序进行排列，然后横向读取


## 1. 找规律做
#### 实际耗时：39ms
```
public String convert(String s, int numRows) {
        StringBuilder sb = new StringBuilder();
        int len = s.length();
        if (0 == len) {
            return "";
        }
        if (numRows < 1) {
            return "";
        }
        if (1 == numRows) {
            return s;
        }
        if (2 == numRows) {
            int i = 0;
            while (i < len) {
                sb.append(s.charAt(i));
                i = i + 2;
            }
            i = 1;
            while (i < len) {
                sb.append(s.charAt(i));
                i = i + 2;
            }
            return sb.toString();
        }
        int group = 2 * numRows - 2;
        int cur;
        //第一行
        for (int j = 0; j < len; j = j + group) {
            sb.append(s.charAt(j));
        }
        for (int i = 1; i < numRows - 1; i++) {
            cur = i * 2;
            for (int j = i; j < len; j = j + cur) {
                cur = group - cur;
                sb.append(s.charAt(j));
            }
        }
        //最后一行
        for (int j = numRows - 1; j < len; j = j + group) {
            sb.append(s.charAt(j));
        }
        return sb.toString();
    }
```
&emsp;&emsp;就是自己画一画图画，可以很容易发现规律，第一行和最后一行的每个数字都差`2*(numRows)`，而中间的数字则是差值在两个值进行变化，而且这两个值加起来就是`2*(numRows)`。
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------
