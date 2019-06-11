> Given a string S and a string T, find the minimum window in S which will contain all the characters in T in complexity O(n).
#### Example：
> Input: S = "ADOBECODEBANC", T = "ABC"
Output: "BANC"
#### Note：
> If there is no such window in S that covers all characters in T, return the empty string "".
If there is such window, you are guaranteed that there will always be only one unique minimum window in S.

#### 解释下题目：
> 找到S中能够最小匹配T的子串


## 1. 滑动窗口
#### 实际耗时：29ms
```
public String minWindow(String s, String t) {
        if (s == null || s.length() < t.length() || s.length() == 0) {
            return "";
        }
        HashMap<Character, Integer> map = new HashMap<>();

        //获得t中的字母出现的次数
        for (char c : t.toCharArray()) {
            if (map.containsKey(c)) {
                map.put(c, map.get(c) + 1);
            } else {
                map.put(c, 1);
            }
        }

        int left = 0;
        int minLeft = 0;
        int minLen = s.length() + 1;
        int count = 0;

        for (int right = 0; right < s.length(); right++) {
            if (map.containsKey(s.charAt(right))) {
                //如果s中有t的字母
                map.put(s.charAt(right), map.get(s.charAt(right)) - 1);
                if (map.get(s.charAt(right)) >= 0) {
                    count++;
                    //System.out.println("count = " + count);
                }

                //窗口已经符合要求了，可以移动了
                while (count == t.length()) {
                    if (right - left + 1 < minLen) {
                        minLeft = left;
                        minLen = right - left + 1;
                    }
                    if (map.containsKey(s.charAt(left))) {
                        map.put(s.charAt(left), map.get(s.charAt(left)) + 1);
                        if (map.get(s.charAt(left)) > 0) {
                            count--;
                        }
                    }
                    left++;
                }
            }
        }
        if (minLen > s.length()) {
            return "";
        }

        return s.substring(minLeft, minLeft + minLen);
    }
```
###### 踩过的坑：注意这里用的是count==t.length()来判断是不是满足要求的，所以需要确保每一个都有。
&emsp;&emsp;思路就是滑动窗口的思想，首先从左到右找，找到一个符合要求的子串，这里用的是hashmap，然后从左边删去一个，并让右边的继续寻找，直到找到左边删去的那个，看看能否使长度缩短，不断这么做即可
###### 时间复杂度O(2n)
###### 空间复杂度O(n)
---------
