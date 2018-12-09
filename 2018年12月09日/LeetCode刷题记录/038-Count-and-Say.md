> The count-and-say sequence is the sequence of integers with the first five terms as following:
1.     1
2.     11
3.     21
4.     1211
5.     111221
`1` is read off as `"one 1"` or `11`.
`11` is read off as `"two 1s"` or `21`.
`21` is read off as `"one 2, then one 1"` or `1211`.
#### Example：
> Input: 1
Output: "1"

> Input: 4
Output: "1211"

#### 解释下题目：
> 这道题目看了半天没看懂，其实就是说让你读前面一串数字，比如5=111221，意思就是代表4的那一串有一个1(`11`)，一个2(`12`)和两个1(`21`)


## 1. 递归
#### 实际耗时：2ms
```
public String countAndSay(int n) {
        if (n <= 0) {
            return "";
        }
        if (1 == n) {
            return "1";
        }
        if (2 == n) {
            return "11";
        } else {
            //递归实现
            String str = countAndSay(n - 1);
            StringBuilder stringBuilder = new StringBuilder();
            char[] a = str.toCharArray();
            int count = 1;
            for (int i = 0; i < a.length - 1; i++) {
                if (a[i] == a[i + 1]) {
                    count++;
                } else {
                    stringBuilder.append(count).append(a[i]);
                    count = 1;
                }
            }
            stringBuilder.append(count).append(a[a.length - 1]);
            return stringBuilder.toString();
        }
    }
```
&emsp;&emsp;思路很简单，就是老老实实读呗。
###### 时间复杂度O(n^2)
###### 空间复杂度O(n)
---------
