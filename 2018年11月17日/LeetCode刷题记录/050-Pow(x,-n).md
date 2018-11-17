> Implement [pow(*x*, *n*)](http://www.cplusplus.com/reference/valarray/pow/), which calculates *x* raised to the power *n* (x<sup>n</sup>).

#### Example：
> Input: 2.00000, 10
Output: 1024.00000

> Input: 2.10000, 3
Output: 9.26100

> Input: 2.00000, -2
Output: 0.25000
Explanation: 2^-2 = 1/2^2 = 1/4 = 0.25
#### Note：
> -100.0 < x < 100.0
n is a 32-bit signed integer, within the range [−2^31, 2^31 − 1]

#### 解释下题目：
> 自己实现幂乘函数


## 1. 一步一步乘
#### 实际耗时：超时
```
public double myPow(double x, int n) {
        double res = 1;
        boolean isNegative = false;
        if (n == 0) {
            return 1;
        } else if (n > 0) {
            isNegative = false;
        } else {
            isNegative = true;
            n = -n;
        }

        for (int i = 0; i < n; i++) {
            res = res * x;
        }

        if (isNegative) {
            return 1.00000 / res;
        } else {
            return res;
        }
    }
```
&emsp;&emsp;思路很简单，就是一次一次乘呗，但是很显然没过去，死在了幂很大的情况下。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
## 2. 二分法
#### 实际耗时：14ms
```
public double myPow(double x, int n) {
        if (n == 0)
            return 1;
        if (n < 0) {
            if (n == Integer.MIN_VALUE) {
                n = Integer.MAX_VALUE - 1;
            } else {
                n = -n;
            }
            x = 1 / x;
        }
        return (n % 2 == 0) ? myPow(x * x, n / 2) : x * myPow(x * x, n / 2);
    }
```
###### 踩过的坑：2.00000^-2147483648 和-1.00000^-2147483648
&emsp;&emsp;思路就是简单的二分法，并不难。但是恶心的是因为-2147483648这个负数，首先将它乘负一将会超越int的范围，然后我想对这个单独处理，结果LeetCode居然还设置了-1的情况，然后不得已我又减了一个1，确保两者奇偶性一致。需要注意的是，在超高精度的时候千万不能这么做
###### 时间复杂度O(logn)
###### 空间复杂度O(logn)
---------
