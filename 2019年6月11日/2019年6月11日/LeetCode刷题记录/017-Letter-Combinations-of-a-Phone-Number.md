> Given a string containing digits from `2-9` inclusive, return all possible letter combinations that the number could represent.
A mapping of digit to letters (just like on the telephone buttons) is given below.
![解释图](http://upload-images.jianshu.io/upload_images/13050335-7211917d9d083688.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### Example：
> Input: "23"
Output: ["ad", "ae", "af", "bd", "be", "bf", "cd", "ce", "cf"].
#### Note：
> 1 does not map to any letters.

#### 解释下题目：
> 按照电话盘的数字对应的字母，列出所有的组合


## 1. 递归
#### 实际耗时：1ms
```
public List<String> letterCombinations(String digits) {
        List<String> result = new ArrayList<>();
        if (0 == digits.length()) {
            return result;
        }
        String[] map = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        recursion("", 0, digits, result, map);
        return result;
    }

    private void recursion(String curString, int curIndex, String digits, List<String> result, String[] map) {
        //curString是目前的组合，还没有到底，curIndex是目前digits中的下标
        if (curIndex == digits.length() - 1) {
            for (int i = 0; i < map[digits.charAt(curIndex) - '0'].length(); i++) {
                String finalString = curString + map[digits.charAt(curIndex) - '0'].charAt(i);
                result.add(finalString);
            }
            return;
        }
        for (int i = 0; i < map[digits.charAt(curIndex) - '0'].length(); i++) {
            recursion(curString + map[digits.charAt(curIndex) - '0'].charAt(i), curIndex + 1, digits, result, map);
        }
    }
```
###### 踩过的坑：空的时候要返回什么都没有，不能返回Null
&emsp;&emsp;思路就是跟全排列一样。搞清楚数组间的关系，头脑清晰，这个很简单。
###### 时间复杂度O(4^n) 这是上限，其实是有多少种组合就是多少
###### 空间复杂度O(1)
---------
