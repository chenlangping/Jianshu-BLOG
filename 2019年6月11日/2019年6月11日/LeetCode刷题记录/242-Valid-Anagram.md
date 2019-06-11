> Given two strings s and t , write a function to determine if t is an anagram of s.
#### Example：
> Input: s = "anagram", t = "nagaram"
Output: true

> Input: s = "rat", t = "car"
Output: false
#### Note：
> You may assume the string contains only lowercase alphabets.

#### 解释下题目：
> anagram这个词的意思就是相同字母异序词。嗯就是这个意思


## 1. 利用数组存
#### 实际耗时：8ms
```
public boolean isAnagram(String s, String t) {
        if (s.length() != t.length()) {
            //长度都不相同还怎么可能是
            return false;
        }
        int[] arr = new int[26];

        for (int i = 0; i < s.length(); i++) {
            arr[s.charAt(i) - 'a']++;
            arr[t.charAt(i) - 'a']--;
        }
        for (int i = 0; i < s.length(); i++) {
            if (arr[s.charAt(i) - 'a'] != 0) {
                return false;
            }
        }

        return true;
    }
```
&emsp;&emsp;开辟一个长度为26的数组，然后如果有对应的字母就加呗，然后另外一个字符串是减操作。如果最后有任何一个位置不是0，说明必定不匹配。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
## 2. 对字符串排序
代码无！但是别这么做，排序真的是万不得已才这么做的，nlogn呀！
