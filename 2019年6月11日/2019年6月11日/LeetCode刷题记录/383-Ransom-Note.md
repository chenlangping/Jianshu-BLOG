> Given an arbitrary ransom note string and another string containing letters from all the magazines, write a function that will return true if the ransom note can be constructed from the magazines ; otherwise, it will return false.
Each letter in the magazine string can only be used once in your ransom note.
#### Example：
> canConstruct("a", "b") -> false
canConstruct("aa", "ab") -> false
canConstruct("aa", "aab") -> true
#### Note：
> You may assume that both strings contain only lowercase letters.



#### 解释下题目：
> 看一个字符串能否完全包含另外一个字符串


## 1. python的方法
#### 实际耗时：24ms
```python
    def canConstruct(self, ransomNote, magazine):
        """
        :type ransomNote: str
        :type magazine: str
        :rtype: bool
        """
        for i in set(ransomNote):
            if ransomNote.count(i) > magazine.count(i):
                return False
        return True
```
&emsp;&emsp;思路很简单，就是Python的count以前没接触过，用起来挺方便的
###### 时间复杂度O(n+m)
###### 空间复杂度O(1)
---------
