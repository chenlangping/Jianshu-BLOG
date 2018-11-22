> Implement `int sqrt(int x)`.
Compute and return the square root of *x*, where x is guaranteed to be a non-negative integer.
Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.
#### Example：
> Input: 4
Output: 2

> Input: 8
Output: 2
Explanation: The square root of 8 is 2.82842..., and since 
             the decimal part is truncated, 2 is returned.


#### 解释下题目：
> 实现开平方根的函数


## 1. 二分法
#### 实际耗时：22ms
```
public int mySqrt(int x) {
        if (0 == x) {
            return 0;
        }
        int small = 0;
        int big = Integer.MAX_VALUE;
        while (true) {
            int mid = small + ((big - small) >> 1);
            if (mid > x / mid) {
                big = mid - 1;
            } else if (mid + 1 > x / (mid + 1)) {
                return mid;
            } else {
                small = mid + 1;
            }
        }
    }
```
&emsp;&emsp;思路没什么好说的，就是二分法判断呗
###### 时间复杂度O(log n )
###### 空间复杂度O(1)
---------
