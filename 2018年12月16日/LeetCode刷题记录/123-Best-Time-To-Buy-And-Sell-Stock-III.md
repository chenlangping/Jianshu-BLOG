> Say you have an array for which the number i element is the price of a given stock on day i.
Design an algorithm to find the maximum profit. You may complete at most two transactions.
#### Example：
> Input: [3,3,5,0,0,3,1,4]
Output: 6
Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3-0 = 3.
             Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 4-1 = 3.

> Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.
#### Note：
> You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).

#### 解释下题目：
> 做两笔交易，获得最大的收益



## 1. 动态规划
#### 实际耗时：3ms
```
public int maxProfit(int[] prices) {
        int buy1 = Integer.MIN_VALUE, buy2 = Integer.MIN_VALUE;
        int sell1 = 0, sell2 = 0;
        for (int i : prices) {
            //如果我们进行了第二次出售可以获得的总钱数
            sell2 = Math.max(sell2, buy2 + i);
            //如果我们进行了第二次购入可以获得的总钱数
            buy2 = Math.max(buy2, sell1 - i);

            //---------------分割线---------------------

            //如果我们进行了第一次出售可以获得的总钱数
            sell1 = Math.max(sell1, buy1 + i);
            //如果我们进行了第一次购入可以获得的总钱数
            buy1 = Math.max(buy1, -i);
        }
        return sell2;
    }
```
&emsp;&emsp;思路在注释里，很好懂
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
