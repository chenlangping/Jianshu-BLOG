> Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.
#### Example：
> Input: `"(()"`
Output: 2
Explanation: The longest valid parentheses substring is `"()"`

> Input: `")()())"`
Output: 4
Explanation: The longest valid parentheses substring is `"()()"`

#### 解释下题目：
> 求给定字符串中最长的匹配上的括号的长度


## 1. 利用stack
#### 实际耗时：34ms
```
public int longestValidParentheses(String s) {
        int max = 0;
        int pos;
        int count = 0;
        int[] array = new int[s.length()];
        Stack<Integer> stack = new Stack();
        //将1设置为匹配上的
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) == '(') {
                stack.push(i);
            } else {
                if (stack.isEmpty()) {
                    continue;
                }
                pos = stack.pop();
                array[pos] = 1;
                array[i] = 1;
            }
        }
        
        //统计数组中连续的1的最多的个数
        for (int i = 0; i < array.length; i++) {
            if (array[i] == 0) {
                count = 0;
            } else {
                count++;
                max = Math.max(max, count);
            }
        }
        return max;
    }
```
&emsp;&emsp;思路就是把匹配上的括号打上标签，随后去找最长的那串1即可
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------
