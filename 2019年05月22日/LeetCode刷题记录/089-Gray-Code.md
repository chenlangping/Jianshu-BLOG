> The gray code is a binary numeral system where two successive values differ in only one bit.
Given a non-negative integer *n* representing the total number of bits in the code, print the sequence of gray code. A gray code sequence must begin with 0.
#### Example：
> Input: 2
Output: [0,1,3,2]
Explanation:
00 - 0
01 - 1
11 - 3
10 - 2
For a given n, a gray code sequence may not be uniquely defined.
For example, [0,2,3,1] is also a valid gray code sequence.
00 - 0
10 - 2
11 - 3
01 - 1

> Input: 0
Output: [0]
Explanation: We define the gray code sequence to begin with 0.
             A gray code sequence of n has size = 2n, which for n = 0 the size is 20 = 1.
             Therefore, for n = 0 the gray code sequence is [0].

#### 解释下题目：
> 在一组数的编码中，若任意两个相邻的代码只有一位二进制数不同，则称这种编码为格雷码。生成格雷码。我记得通信概论里面还学过这个。。


## 1. 发现规律做
#### 实际耗时：1ms
```
public List<Integer> grayCode(int n) {
        List<Integer> res = new ArrayList<>();
        if (n < 0) {
            return res;
        } else if (n == 0) {
            res.add(0);
            return res;
        } else if (n == 1) {
            res.add(0);
            res.add(1);
            return res;
        } else {
            res.add(0);
            res.add(1);
            for (int i = 1; i < n; i++) {
                int tmp = (int) Math.pow(2, i);
                for (int j = tmp - 1; j >= 0; j--) {
                    res.add(res.get(j) + tmp);
                }
            }
            return res;
        }
    }
```
&emsp;&emsp;思路就是自己画个格雷码找规律就行了。而且规律还不止一种
###### 时间复杂度O(2^n)
###### 空间复杂度O(2^n)
---------
