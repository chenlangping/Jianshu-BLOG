> Given a string containing just the characters `'('`, `')'`, `'{'`, `'}'`, `'['` and `']'`, determine if the input string is valid.
> An input string is valid if:
> * Open brackets must be closed by the same type of brackets.
> * Open brackets must be closed in the correct order.
#### Example：
> Input: "()"&emsp;&emsp;Output: true
> Input: "()[]{}"&emsp;&emsp;Output: true
>Input: "(]"&emsp;&emsp;Output: false
> Input: "([)]"&emsp;&emsp;Output: false
> Input: "{[]}"&emsp;&emsp;Output: true
#### Note：
> Note that an empty string is also considered valid.

#### 解释下题目：
> 检查下括号是否匹配，空的也算匹配。


## 1. 利用StringBuider当做栈来处理
#### 实际耗时：8ms
```
public boolean isValid(String s) {
        if (s.equals("")) {
            return true;
        }
        int length = s.length();
        if (length % 2 != 0) {
            //奇数个是不可能匹配的
            return false;
        }
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < length; i++) {
            if ((s.charAt(i) == '(') || (s.charAt(i) == '[') || (s.charAt(i) == '{')) {
                sb.append(s.charAt(i));
            } else {
                if (0 == sb.length()) {
                    return false;
                } else {
                    //剩下的三个右括号
                    if (s.charAt(i) == ')' && sb.charAt(sb.length() - 1) == '(') {
                        sb.deleteCharAt(sb.length() - 1);
                    } else if (s.charAt(i) == ']' && sb.charAt(sb.length() - 1) == '[') {
                        sb.deleteCharAt(sb.length() - 1);
                    } else if (s.charAt(i) == '}' && sb.charAt(sb.length() - 1) == '{') {
                        sb.deleteCharAt(sb.length() - 1);
                    }
                }
            }
        }
        if (0 == sb.length()) {
            return true;
        } else return false;
    }
```
###### 踩过的坑：
`"(([]){})"`  应该是`true`

&emsp;&emsp;利用栈的思想，如果是左括号就入栈，如果是右括号就弹出一个来看看匹不匹配，不匹配就直接错误，否则就删除这一对，直到判断完整个数组。最后如果栈中没有内容说明匹配。当然我是用StringBuilder模拟stack，具体见方法2
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
## 2. 正统的Stack方法
#### 实际耗时：5ms
代码从[这里](https://leetcode.com/problems/valid-parentheses/discuss/9178/Short-java-solution)看到的，很秀。
```
public boolean isValid2(String s) {
        Stack<Character> stack = new Stack<Character>();
        for (char c : s.toCharArray()) {
            if (c == '(') {
                stack.push(')');
            } else if (c == '{') {
                stack.push('}');
            } else if (c == '[') {
                stack.push(']');
            } else if (stack.isEmpty() || stack.pop() != c) {
                return false;
            }
        }
        return stack.isEmpty();
    }
```
&emsp;&emsp;思路大体上跟1一样，只不过该作者进行了改良，是检测到左括号就放入相应的右括号，然后右括号就弹出看看一不一样，不一样就错。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
