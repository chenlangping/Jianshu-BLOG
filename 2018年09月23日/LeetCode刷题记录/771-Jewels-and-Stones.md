> You're given strings `J` representing the types of stones that are jewels, and `S` representing the stones you have.  Each character in `S` is a type of stone you have.  You want to know how many of the stones you have are also jewels.
The letters in `J` are guaranteed distinct, and all characters in `J` and `S` are letters. Letters are case sensitive, so `"a"` is considered a different type of stone from `"A"`.
#### Example：
> Input: J = "aA", S = "aAAbbbb"&emsp;&emsp;Output: 3
Input: J = "z", S = "ZZ"&emsp;&emsp;Output: 0
#### Note：
> `S` and `J` will consist of letters and have length at most 50.
The characters in `J` are distinct.

#### 解释下题目：
> 给定一个不会重复的字符串`J`，找出`J`中的所有字符在`S`中出现了的次数总和


## 1. 暴力呗，不然还有什么方法
#### 实际耗时：11ms
```
public int numJewelsInStones(String J, String S) {
        int sum = 0;
        for (int i = 0; i < J.length(); i++) {
            for (int j = 0; j < S.length(); j++) {
                if (J.charAt(i) == S.charAt(j)) {
                    sum++;
                }
            }
        }
        return sum;
    }
```
&emsp;&emsp;把J里面的取出来，在S中进行匹配查找。就那么简单。
###### 时间复杂度O(mn)，mn分别为数组的长度
###### 空间复杂度O(1)
---------
## 2. 利用Java自带的正则表达式取代
#### 实际耗时：25ms
```
public int numJewelsInStones2(String J, String S) {
        return S.replaceAll("[^" + J + "]", "").length();
    }
```
&emsp;&emsp;把S中所有的J的那些字符都删掉(用""替代)，剩下的长度就是J中所有字符了，计算长度即可。
###### 时间复杂度未知，但是我觉得应该比暴力快才对
###### 空间复杂度O(1)
---------
## 3. 利用HashMap/HashSet
#### 实际耗时：23ms[应该是系统问题才那么慢]
```
public int numJewelsInStones3(String J, String S) {
        Set set = new HashSet();
        int sum = 0;
        for (char j : J.toCharArray()) {
            set.add(j);
        }
        for (char s : S.toCharArray()) {
            if (set.contains(s)) {
                sum++;
            }
        }
        return sum;
    }
```
&emsp;&emsp;emmm这么简单应该谁都看得懂
###### 时间复杂度O(n+m) mn分别为数组的长度
###### 空间复杂度O(1)
---------
