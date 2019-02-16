> Given two integers `dividend` and `divisor`, divide two integers without using multiplication, division and mod operator.
Return the quotient after dividing `dividend` by `divisor`.
The integer division should truncate toward zero.
#### Example：
> Input: dividend = 10, divisor = 3
Output: 3

> Input: dividend = 7, divisor = -3
Output: -2
#### Note：
> Both dividend and divisor will be`32-bit` signed integers.
The divisor will never be `0`.
Assume we are dealing with an environment which could only store integers within the 32-bit signed integer range: [−2^31,  2^31 − 1]. For the purpose of this problem, assume that your function returns 2^31 − 1 when the division result overflows.

#### 解释下题目：
> 实现计算机除法，用int足够了，而且除数不会是0


## 1. 老实做
#### 实际耗时：超时
```
public int divide(int dividend, int divisor) {
        int result = 0;
        if (0 == dividend) {
            return 0;
        }
        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            return Integer.MAX_VALUE;
        }
        
        if (dividend > 0 && divisor > 0) {
            //相当于 10/3
            while (dividend >= 0) {
                dividend -= divisor;
                result++;
            }
            result--;
        } else if (dividend < 0 && divisor > 0) {
            //相当于 -10/3
            while (dividend <= 0) {
                dividend += divisor;
                result--;
            }
            result++;
        } else if (dividend > 0 && divisor < 0) {
            //相当于 10/-3
            while (dividend >= 0) {
                dividend += divisor;
                result--;
            }
            result++;
        } else {
            //相当于 -10/-3
            while (dividend <= 0) {
                dividend -= divisor;
                result++;
            }
            result--;
        }
        return result;
    }
```
&emsp;&emsp;思路很简单，就是把除数化为减法，不停去做就行了。当然这么暴力，显然没过。。。。
###### 时间复杂度O(a)  a=答案
###### 空间复杂度O(1)
---------
## 2. 类似二分法的思想
#### 实际耗时：xxms
```
public int divide(int dividend, int divisor) {
        if (dividend == Integer.MIN_VALUE && divisor == -1) {
            return Integer.MAX_VALUE;
        }
        if (dividend > 0 && divisor > 0) {
            return divideHelper(-dividend, -divisor);
        } else if (dividend > 0) {
            return -divideHelper(-dividend, divisor);
        } else if (divisor > 0) {
            return -divideHelper(dividend, -divisor);
        } else {
            return divideHelper(dividend, divisor);
        }
    }

    private int divideHelper(int dividend, int divisor) {
        //首先处理被除数小于除数的问题
        if (divisor < dividend) {
            return 0;
        }
        //得到可以除的最大的
        int cur = 0, res = 0;
        while ((divisor << cur) >= dividend && divisor << cur < 0 && cur < 31) {
            cur++;
        }
        res = dividend - (divisor << cur - 1);
        if (res > divisor) {
            return 1 << cur - 1;
        }
        return (1 << cur - 1) + divide(res, divisor);
    }
```
&emsp;&emsp;思路就是我先不断扩大除数，然后就把这个最大的除数去除被除数，剩下的余数我在用相同的方法去做，把它们相加即可。
###### 时间复杂度O(logn)
###### 空间复杂度O(1)
---------
