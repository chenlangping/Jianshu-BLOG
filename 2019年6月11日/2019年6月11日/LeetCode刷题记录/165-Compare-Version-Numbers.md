> Compare two version numbers version1 and version2.
If `version1` > `version2` return `1`; if `version1` < `version2` return `-1`;otherwise return `0`.
You may assume that the version strings are non-empty and contain only digits and the `.`character.
The `.` character does not represent a decimal point and is used to separate number sequences.
For instance, `2.5` is not "two and a half" or "half way to version three", it is the fifth second-level revision of the second first-level revision.
You may assume the default revision number for each level of a version number to be `0`. For example, version number `3.4` has a revision number of `3` and `4` for its first and second level revision number. Its third and fourth level revision number are both `0`.
#### Example：
> Input: version1 = "0.1", version2 = "1.1"
Output: -1

> Input: version1 = "1.0.1", version2 = "1"
Output: 1

> Input: version1 = "7.5.2.4", version2 = "7.5.3"
Output: -1

> Input: version1 = "1.01", version2 = "1.001"
Output: 0
Explanation: Ignoring leading zeroes, both “01” and “001" represent the same number “1”

> Input: version1 = "1.0", version2 = "1.0.0"
Output: 0
Explanation: The first version number does not have a third level revision number, which means its third level revision number is default to "0"
#### Note：
> Version strings are composed of numeric strings separated by dots . and this numeric strings may have leading zeroes.
Version strings do not start or end with dots, and they will not be two consecutive dots.

#### 解释下题目：
> 给定两个版本号的字符串，比较它们的大小


## 1. 分割然后比较呗
#### 实际耗时：1ms
```
public int compareVersion(String version1, String version2) {
        String[] v1 = version1.split("\\.");
        String[] v2 = version2.split("\\.");
        int len = Math.min(v1.length, v2.length);

        for (int i = 0; i < len; i++) {
            int a = Integer.valueOf(v1[i]);
            int b = Integer.valueOf(v2[i]);
            if (a > b) {
                return 1;
            } else if (a < b) {
                return -1;
            }
        }
        if (v1.length == v2.length) {
            return 0;
        } else if (v1.length < v2.length) {
            for (int i = v1.length; i < v2.length; i++) {
                int a = Integer.valueOf(v2[i]);
                if (a > 0) {
                    return -1;
                }
            }
            return 0;
        } else {
            for (int i = v2.length; i < v1.length; i++) {
                int a = Integer.valueOf(v1[i]);
                if (a > 0) {
                    return 1;
                }
            }
            return 0;
        }
    }
```
&emsp;&emsp;思路其实很简单，分割比较呗。这里我感觉要注意的知识点有两个：
- 一个是点号作为分割符的话需要两个转义字符，即`String[] v1 = version1.split("\\.");`
- 还有一个是`Integer.valueOf()`和`Integer.parseInt()`这两个函数，它们都可以以字符串作为参数，不同的是value这个返回Integer对象，而parseInt这个返回的是基本类型int，所以如果了解包装类的话，这个就很好理解了。
###### 时间复杂度O(n)  n为较长的那个字符串的长度
###### 空间复杂度O(1)
---------
