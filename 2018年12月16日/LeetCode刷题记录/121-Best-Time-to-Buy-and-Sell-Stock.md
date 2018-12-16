> Say you have an array for which the ith element is the price of a given stock on day i.
If you were only permitted to complete at most one transaction (i.e., buy one and sell one share of the stock), design an algorithm to find the maximum profit.
#### Example：
> Input: [7,1,5,3,6,4]
Output: 5
Explanation: Buy on day 2 (price = 1) and sell on day 5 (price = 6), profit = 6-1 = 5.
             Not 7-1 = 6, as selling price needs to be larger than buying price.

> Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.

#### Note：
> you cannot sell a stock before you buy one.

#### 解释下题目：
> 给定一个数组，这个数组中每个元素的值是每天的股价，计算最大的获利情况。


## 1. 暴力破解
#### 实际耗时：386ms
```
public int maxProfit(int[] prices) {
        int max = 0;
        for (int i = 0; i < prices.length; i++) {
            for (int j = i + 1; j < prices.length; j++) {
                if (prices[j] > prices[i]) {
                    max = Math.max(max, prices[j] - prices[i]);
                }
            }
        }
        return max;
    }
```
&emsp;&emsp;针对每一个数字，从它开始往后面找，找到比它大的就计算下如果在这一天卖出去能赚多少。显然很复杂。
###### 时间复杂度O(n^2)
###### 空间复杂度O(1)
---------
## 2. Kadane算法
#### 实际耗时：3ms
```
public int maxProfit2(int[] prices) {
        //maxCur用来记录当前序列中(不一定是从0开始的)最大的，maxSoFar用来记录从头到目前为止最大的
        int maxCur = 0;
        int maxSoFar = 0;
        for (int i = 1; i < prices.length; i++) {
            //为什么当maxCur<0的时候要把它置成0呢？相当于是找到一个比目前最小的还要小的了，需要从头开始的意思
            maxCur = Math.max(0, maxCur += prices[i] - prices[i - 1]);
            maxSoFar = Math.max(maxCur, maxSoFar);
        }
        return maxSoFar;
    }
```
&emsp;&emsp;这道题目其实可以转换成求解一个数组中寻找最大子数列的问题，股票价格：`[7,1,5,3,6,4]`-->每两天的差值： `[-6,4,-2,3,-2]` 相当于找出最大的子数列，明显是`4+ -2 +3 =5` 
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
