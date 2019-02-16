> Implement [strStr()](http://www.cplusplus.com/reference/cstring/strstr/).
Return the index of the first occurrence of needle in haystack, or **-1** if needle is not part of haystack.

#### Example：
> Input: haystack = "hello", needle = "ll"
Output: 2

> Input: haystack = "aaaaa", needle = "bba"
Output: -1
#### Clarification：
> What should we return when `needle` is an empty string? This is a great question to ask during an interview.
For the purpose of this problem, we will return 0 when `needle` is an empty string. This is consistent to C's [strstr()](http://www.cplusplus.com/reference/cstring/strstr/) and Java's [indexOf()](https://docs.oracle.com/javase/7/docs/api/java/lang/String.html#indexOf(java.lang.String)).


#### 解释下题目：
> 实现java的indexOf()方法


## 1. 挨个判断
#### 实际耗时：6ms
```
public int strStr(String haystack, String needle) {
        if (needle.equals("") || null == needle) {
            return 0;
        }

        //最多循环haystack.length() - needle.length()次
        for (int i = 0; i <= haystack.length() - needle.length(); i++) {
            int now = i; //记录目前的haystack中的索引
            int count = 0; //匹配上的个数
            for (int j = 0; j < needle.length(); j++) {
                if (haystack.charAt(now) != needle.charAt(j)) {
                    break;
                } else {
                    count++;
                    now++;
                }
                if (count == needle.length()) {
                    return i;
                }
            }
        }
        return -1;
    }
```

&emsp;&emsp;思路：那除了挨个判断还能怎么做呢？
###### 时间复杂度O(nm)  n和m分别为两个数组的长度
###### 空间复杂度O(1)
---------

