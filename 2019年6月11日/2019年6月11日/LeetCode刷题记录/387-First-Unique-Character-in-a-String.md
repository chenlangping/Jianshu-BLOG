> Given a string, find the first non-repeating character in it and return it's index. If it doesn't exist, return -1.
#### Example：
> s = "leetcode"
return 0

> s = "loveleetcode",
return 2
#### Note：
> 题目里没说，但是做的时候发现是默认全部都是小写

#### 解释下题目：
> 就是找出第一个只出现一次的字符的下标，如果没有，则返回-1


## 1. python的列表生成式
#### 实际耗时：52ms
```python
    def firstUniqChar(self, s):
        """
        :type s: str
        :rtype: int
        """
        index = [i for i in string.lowercase if 1 == s.count(i)]
        if len(index) == 0:
            return -1
        for i in s:
            if i in index:
                return s.index(i)
```
&emsp;&emsp;普通的思路就是统计每个单词出现的次数，然后找出那个出现次数为1的，而且是最早出现在字符串中的就行了。这里只贴了python的解法，因为列表生成式真的挺方便的，这里直接在index中填入了只出现一次的字符，然后判断即可。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
