> Given a string of numbers and operators, return all possible results from computing all the different possible ways to group numbers and operators. The valid operators are `+`, `-` and `*`.
#### Example：
> 题目举例
#### Note：
> Input: "2-1-1"
Output: [0, 2]
Explanation: 
((2-1)-1) = 0 
(2-(1-1)) = 2

> Input: "2*3-4*5"
Output: [-34, -14, -10, -10, 10]
Explanation: 
(2*(3-(4*5))) = -34 
((2*3)-(4*5)) = -14 
((2*(3-4))*5) = -10 
(2*((3-4)*5)) = -10 
(((2*3)-4)*5) = 10

#### 解释下题目：
> 给定一个表达式，只有数字和加减乘三种运算符，让你加括号求解出不同的值


## 1. Devide and Conquer
#### 实际耗时：4ms
```
public List<Integer> diffWaysToCompute(String input) {
        boolean hasOperator = false;
        //用来记录当前的String中是否有运算符，如果没有就说明这个String是一个单纯的数字
        List<Integer> result = new ArrayList<>();
        List<Integer> left = new ArrayList<>();
        List<Integer> right = new ArrayList<>();
        if (0 == input.length()) {
            return result;
        }
        for (int i = 0; i < input.length(); i++) {
            if (input.charAt(i) == '+' || input.charAt(i) == '-' || input.charAt(i) == '*') {
                hasOperator = true;
                //左边的n个数
                left = diffWaysToCompute(input.substring(0, i));
                //右边的n个数
                right = diffWaysToCompute(input.substring(i + 1));

                //根据它们的符号进行相应的操作
                if (input.charAt(i) == '+') {
                    for (int j = 0; j < left.size(); j++) {
                        for (int k = 0; k < right.size(); k++) {
                            result.add(left.get(j) + right.get(k));
                        }
                    }
                } else if (input.charAt(i) == '-') {
                    for (int j = 0; j < left.size(); j++) {
                        for (int k = 0; k < right.size(); k++) {
                            result.add(left.get(j) - right.get(k));
                        }
                    }
                } else {
                    for (int j = 0; j < left.size(); j++) {
                        for (int k = 0; k < right.size(); k++) {
                            result.add(left.get(j) * right.get(k));
                        }
                    }
                }
            }
        }

        if (!hasOperator) {
            //纯数字，返回
            result.add(Integer.valueOf(input));
        }
        return result;
    }
```
&emsp;&emsp;思路就是如果找到一个运算符，那么递归可以找到左边的答案（多个），同理也可以找到右边的答案，最后根据这个运算符左右两边计算即可。需要注意的出口条件是这个String是一个纯数字的情况下。
###### 时间复杂度O(3^n)
|表达式|时间|
|:--|:--|
|1|1|
|T(1) + T(1)|2|
| (T(1) + T(2)) + (T(2) + T(1))                  | 6
| (T(1) + T(3)) + (T(2) + T(2)) + (T(3) + T(1))  | 18 
###### 空间复杂度O(1)
---------
