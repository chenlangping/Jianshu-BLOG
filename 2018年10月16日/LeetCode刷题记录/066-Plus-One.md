> Given a **non-empty** array of digits representing a non-negative integer, plus one to the integer.
The digits are stored such that the most significant digit is at the head of the list, and each element in the array contain a single digit.
You may assume the integer does not contain any leading zero, except the number 0 itself.
#### Example：
> Input: [1,2,3]
Output: [1,2,4]
Explanation: The array represents the integer 123.

> Input: [4,3,2,1]
Output: [4,3,2,2]
Explanation: The array represents the integer 4321.

#### 解释下题目：
> 给定一个数组，这个数组代表了一个数字，将这个数字加一，然后输出


## 1. 倒置判断
#### 实际耗时：0ms
```
public int[] plusOne(int[] digits) {
        int length = digits.length;
        //从最后一位开始
        for (int i = length - 1; i >= 0; i--) {
            //如果不存在进位
            if (digits[i] < 9) {
                digits[i]++;
                return digits;
            }
            //如果存在进位
            digits[i] = 0;
        }
        //如果一直进位到第一位，那么需要重新搞一个新的
        int[] newResult = new int[length + 1];
        newResult[0] = 1;
        return newResult;
    }
```
&emsp;&emsp;从最后一位开始判断，因为只是加一，所以可以肯定的是只要有一位开始不进位，那么之后的都不可能进位。这个容易陷入进位的思维陷阱中，其实只要确保加一这个操作只执行一次就行。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------




