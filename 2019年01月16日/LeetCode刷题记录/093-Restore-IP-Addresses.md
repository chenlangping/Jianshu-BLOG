> Given a string containing only digits, restore it by returning all possible valid IP address combinations.
#### Example：
> Input: "25525511135"
Output: ["255.255.11.135", "255.255.111.35"]


#### 解释下题目：
> 题目很简单，给定一个全是数字的字符串，然后通过加点（3个）使之成为合法的IP地址。


## 1. 三重循环
#### 实际耗时：3ms
```
public List<String> restoreIpAddresses(String s) {
        List<String> res = new ArrayList<>();
        if (s.length() > 12) {
            //最大是255.255.255.255
            return res;
        }
        for (int i = 1; i < 4 && i < s.length() - 2; i++) {
            for (int j = i + 1; j < i + 4 && j < s.length() - 1; j++) {
                for (int k = j + 1; k < j + 4 && k < s.length(); k++) {
                    String s1 = s.substring(0, i), s2 = s.substring(i, j), s3 = s.substring(j, k), s4 = s.substring(k);
                    if (isValidIPAddress(s1) && isValidIPAddress(s2) && isValidIPAddress(s3) && isValidIPAddress(s4)) {
                        res.add(s1.trim() + "." + s2 + "." + s3 + "." + s4);
                    }
                }
            }
        }
        return res;
    }

    private boolean isValidIPAddress(String s) {
        if (s.length() >= 4 || (s.charAt(0) == '0' && s.length() > 1)) {
            return false;
        }
        if (Integer.valueOf(s) <= 255) {
            return true;
        }
        return false;
    }
```
###### 踩过的坑：这里有两个坑，第一个是原来我判断子串是否合法是用的小于等于255，但是我忽略了可能这串会比较长，导致Integer.valueof的值会大于int的范围。还有一个坑是我之前并没有`s.charAt(0) == '0' && s.length() > 1`这个条件，导致我把1.1.1.1和1.1.1.001判断成不一样的了。
&emsp;&emsp;思路很简单，我需要加三个点，那我遍历去"加点"，其实是找出点的位置，然后去取出来判断即可。
###### 时间复杂度O(27)
###### 空间复杂度O(1)
---------
