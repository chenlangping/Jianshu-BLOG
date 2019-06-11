> Write a function that takes a string as input and reverse only the vowels of a string.
#### Example：
> Input: "hello"
Output: "holle"

> Input: "leetcode"
Output: "leotcede"

#### 解释下题目：
> 把字符串里的所有元音字符翻个位子


## 1. “指针”来做
#### 实际耗时：2ms
```
public String reverseVowels(String s) {
        char[] c = s.toCharArray();
        final String vowels = "aeiouAEIOU";
        int j = s.length() - 1;
        for (int i = 0; i < j; i++) {
            if (vowels.indexOf(c[i]) > -1) {
                for (; j >= 0; j--) {
                    if (vowels.indexOf(c[j]) > -1) {
                        char tmp = c[i];
                        c[i] = c[j];
                        c[j] = tmp;
                        j--;
                        break;
                    }
                }
            }
        }

        return String.valueOf(c);
    }
```
###### 踩过的坑：`race car`
&emsp;&emsp;思路没什么好主意的，就是从头和尾部开始找元音，找到了就交换位置呗。
&emsp;&emsp;这里我判断元音本来是写了一个函数，后来看到`python`中可以用`in`来判断，就把判断改成了上面函数的样子。然后不论是python还是Java，都不能直接处理字符串，所以需要转换成字符数组/列表，最后需要转换回来。
Python：
- String->list  `list(str)`
- list->String `"".join(l)`

Java：
- String -> char[]  `str.toCharArray()`
- char[] -> String  `String.valueOf(c)`
###### 时间复杂度O(n) n为字符串长度
###### 空间复杂度O(1)
---------
