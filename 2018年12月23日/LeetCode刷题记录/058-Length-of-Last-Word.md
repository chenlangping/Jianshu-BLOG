> Given a string *s* consists of upper/lower-case alphabets and empty space characters `' '`, return the length of last word in the string.
If the last word does not exist, return 0.
#### Example：
> Input: "Hello World"
Output: 5
#### Note：
>  A word is defined as a character sequence consists of non-space characters only.

#### 解释下题目：
> 给定一个字符串，找出其中最后一个单词（如果存在的话）的长度。


## 1. 字符串判断
#### 实际耗时：4ms
```
public int lengthOfLastWord(String s) {
        //特殊情况处理
        if (s.length() == 0) {
            return 0;
        }
        int result = 0;
        int i = s.length() - 1;
        int j = 0;

        //对最后的空格进行记录
        while (i >= 0 && s.charAt(i) == ' ') {
            i--;
            j++;
        }

        //计算最后一个单词的长度
        while (i >= 0) {
            if (s.charAt(i) == ' ') {
                result = s.length() - i - 1 - j;
                break;
            }
            if (0 == i) {
                result = s.length() - j;
                break;
            }
            i--;
        }
        return result;
    }
```
###### 踩过的坑："" 和 " " 和 "a"
&emsp;&emsp;思路就是老老实实做呗。注意几点：首先是`"hello world    "` 这个答案是5，也就是会忽略最后的空格。其次是第一次开始做的时候注意它可能不包含任何单词，以及可能这个字符串中压根就没空格，注意这两个坑应该就没什么问题了。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
## 2. 正则表达式
#### 实际耗时：5ms
```
public int lengthOfLastWord2(String s) {
        return s.trim().length() - s.trim().lastIndexOf(" ") - 1;
    }
```
&emsp;&emsp;思路：简单粗暴，直接用正则解决即可。
###### 时间复杂度O(不知道)
###### 空间复杂度O(1)
