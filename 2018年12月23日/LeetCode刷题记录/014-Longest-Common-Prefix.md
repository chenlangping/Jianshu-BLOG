> Write a function to find the longest common prefix string amongst an array of strings.
If there is no common prefix, return an empty string `""`.
#### Example：
> Input: ["flower","flow","flight"]       Output: "fl"
   Input: ["dog","racecar","car"]       Output: ""        Explanation: There is no common prefix among the input strings.

#### 解释下题目：
> 返回所给定的所有字符串中的最长的子串


## 1. 简单暴力判断——竖直法
#### 实际耗时：10ms
```
public String longestCommonPrefix(String[] strs) {
        if (strs.length == 0) {
            return "";
        }
        int index = 0;
        while (true) {
            for (String str : strs) {
                if (index == str.length() || str.charAt(index) != strs[0].charAt(index)) {
                    return strs[0].substring(0, index);
                }
                //每一轮比较所有字符串的第index个字符
            }
            index++;
        }
    }
```
&emsp;&emsp;就是简单的暴力破解遍历。其实题目是有点问题的，因为`String[] strs = {"aflower", "flow", "flight"};` 我认为应该返回`fl`，然而其实题目不是这个意思
###### 时间复杂度O(n)  n是整个字符串数组中所有字符的和
###### 空间复杂度O(1)
---------
## 2. 简单暴力判断——水平法
#### 实际耗时：7ms
```
public String longestCommonPrefix2(String[] strs) {
        if (strs.length == 0) return "";
        String result = strs[0];
        for (int i = 1; i < strs.length; i++)
            while (strs[i].indexOf(result) != 0) {
                result = result.substring(0, result.length() - 1);
                if (result.isEmpty()) return "";
            }
        return result;
    }
```
&emsp;&emsp;就是一个子串一个子串进行比较，相当于两两比较，找到最小的子串，然后再去比较。其实和竖直法是同一量级的，但是可能因为测试数据的原因时间略快了一点。
###### 时间复杂度O(n)  n是整个字符串数组中所有字符的和
###### 空间复杂度O(1)
---------
## 3. 递归的方法
#### 实际耗时：未知
代码是我从LeetCode抄来的：
```
public String longestCommonPrefix(String[] strs) {
    if (strs == null || strs.length == 0) return "";    
        return longestCommonPrefix(strs, 0 , strs.length - 1);
}

private String longestCommonPrefix(String[] strs, int l, int r) {
    if (l == r) {
        return strs[l];
    }
    else {
        int mid = (l + r)/2;
        String lcpLeft =   longestCommonPrefix(strs, l , mid);
        String lcpRight =  longestCommonPrefix(strs, mid + 1,r);
        return commonPrefix(lcpLeft, lcpRight);
   }
}

String commonPrefix(String left,String right) {
    int min = Math.min(left.length(), right.length());       
    for (int i = 0; i < min; i++) {
        if ( left.charAt(i) != right.charAt(i) )
            return left.substring(0, i);
    }
    return left.substring(0, min);
}
```
&emsp;&emsp;思路就是不停递归去把字符串数组不断“缩小”，直到小到只剩下两个字符串，然后就能找出这两个字符串的子串，然后不停重复就能求出解。然而这个方法其实并比不上1和2，因为它还需要开辟额外的空间。
###### 时间复杂度O(n)  n是整个字符串数组中所有字符的和
###### 空间复杂度O(m*log(n)) m是字符串数组的长度，n是整个字符串数组中所有字符的和
---------
还有两种方法，都比不上1和2，emmmm就先偷个懒不写了。
