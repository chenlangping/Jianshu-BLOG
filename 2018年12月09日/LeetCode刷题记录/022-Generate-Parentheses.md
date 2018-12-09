> Given *n* pairs of parentheses, write a function to generate all combinations of well-formed parentheses.
#### Example：
> n=3
[
  "((()))",
  "(()())",
  "(())()",
  "()(())",
  "()()()"
]
#### 解释下题目：
> 给定一个数字n，输出n对括号的所有组合


## 1. 递归输出
#### 实际耗时：2ms
```
public List<String> generateParenthesis(int n) {
        List<String> list = new ArrayList<>();
        createParentheses(list, "", 0, n);
        return list;
    }

    //j是左括号有多少个，i是右括号有多少个
    //思路是当j>0时就不断深入，j=0，这意味着左括号用完了，如果i=0 说明也匹配完了，然后就可以写入
    private void createParentheses(List<String> list, String str, int i, int j) {
        if (i == 0 && j == 0) {
            list.add(str);
            return;
        }
        if (j > 0) createParentheses(list, str + "(", i + 1, j - 1);
        if (i > 0) createParentheses(list, str + ")", i - 1, j);
    }
```
&emsp;&emsp;思路就是设目前有n个左括号，那么就肯定需要n个右括号来匹配，我们需要做的就是添加括号的操作，如果括号用完了，就生成了其中的一个解。
###### 时间复杂度O(4^n/根号n) 
###### 空间复杂度O(4^n/根号n)
---------
## 2. 把所有的括号的组合都输出
#### 实际耗时：没做ms
```
这个时间会超级复杂，没写
```
&emsp;&emsp;思路就是我可以在每个位置，要么放左括号，要么放右括号，那我都放试一试，如果是合法的输出我就加到list中。相当暴力，当然时间复杂度会很恐怖。
###### 时间复杂度O(2^2n)
###### 空间复杂度O(1)
---------
