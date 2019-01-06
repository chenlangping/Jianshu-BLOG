> Determine whether an integer is a palindrome. An integer is a palindrome when it reads the same backward as forward.
#### Example:
> Input: 121  Output: true
   Input: -121 Output: false    Explanation: From left to right, it reads -121. From right to left, it becomes 121-. Therefore it is not a palindrome.
   Input: 10     Output: false   Explanation: Reads 01 from right to left. Therefore it is not a palindrome.

#### 解释下题目：
> 就是判断一个数从左右读和从右边读是不是一样的。

## 1. 利用在[007](https://www.jianshu.com/p/73d788502bcf)中的倒置判断
#### 实际耗时：103ms
```
public boolean isPalindrome(int x) {
        try {
            if (x == 0) {
                return true;
            } else if (x < 0) {
                return false;
            } else {
                String num = String.valueOf(x);
                String mnum = new StringBuilder(num).reverse().toString();
                if (num.equals(mnum)) {
                    return true;
                } else {
                    return false;
                }
            }
        } catch (Exception e) {
            return false;
        }
    }
```
小于0是不可能是回文数的，然后还是利用StringBuilder的reverse方法，最后判断下这两个数是否一样即可。这里Builder比Buffer快了三四毫秒的样子。
可能可以增加效率的判断：如果0<=x<10，那么一定是回文数，如果x的尾数是0，那么一定不可能是回文数。
###### 时间复杂度 不知道 空间复杂度O(1)
---------
## 2. 利用取模运算查找
#### 实际耗时：224ms
```
public boolean isPalindrome2(int x) {
        int num = 0;
        if(x < 0 || (x % 10 == 0 && x != 0)) {
            return false;
        }
        if (x < 10) return true;
        while (num < x) {
            num = x % 10 + num * 10;
            x = x / 10;
        }
        if (x == num || x == num / 10) {
            return true;
        }
        return false;
    }
```
####踩过的坑 `10` 
用这种方法需要首先判断`10`这种最后一位是0的数字是不是正确输出。
首先思路就是将数字"一分为二"，然后选择一边进行颠倒操作，最后和剩下的一边进行比较即可。一分为二的操作主要靠一个数字不断减小，另外一个数字不断增大来实现。用这种方法最后`x`可能比`num`小，所以需要判断`x==num /10`来确保判断的准确性，还有一定一定要记得先把末尾是0的数字全部剔除。
###### 时间复杂度O(log(n))  空间复杂度O(1)
