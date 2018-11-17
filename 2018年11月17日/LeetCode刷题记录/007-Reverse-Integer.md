> Given a 32-bit signed integer, reverse digits of an integer.
#### Example:
>  Input: 123   Output: 321
    Input: -123  Output: -321
    Input: 120   Output: 21
    
#### 解释下题目：
> 就是给一个int范围的数字，然后倒置一下它。


## 1. 直接用java StringBuilder的reverse()方法
#### 实际耗时：24ms
```
public int reverse(int x) {
        try{
            if(x==0){
                return 0;
            }else if(x<0){
                //如果是负数
                x=(-1)*x;
                String num = String.valueOf(x);
                String mnum = new StringBuffer(num).reverse().toString();
                int result = Integer.parseInt(mnum);
                return (-1)*result;
            }else {
                //排除0和负数，只剩下正数
                String num = String.valueOf(x);
                String mnum = new StringBuffer(num).reverse().toString();
                return Integer.parseInt(mnum);
            }
        }catch (Exception e){
            return 0;
        }

    }
```
负数转成正数，然后用StringBuffer的reverse方法倒置一下字符串，然后用包装类Integer转换一下字符串成数字即可，如果是负数加上一个符号输出即可。有意思的是，我用StringBuffer居然比StringBuilder快上四五毫秒......
###### 时间复杂度 不知道  空间复杂度O(1)
---------
## 2. 利用栈的先进后出思想实现
#### 实际耗时：45ms
```
public int reverse2(int x) {
        int num = 0;
        while (x != 0) {
            int i = x % 10;
            x /= 10;
            if (num > Integer.MAX_VALUE / 10 || (num == Integer.MAX_VALUE / 10 && i > 7)) return 0;
            if (num < Integer.MIN_VALUE / 10 || (num == Integer.MIN_VALUE / 10 && i < -8)) return 0;
            num = num * 10 + i;
        }
        return num;
    }
```
就是一个一个取出数字，然后再加起来。
唯一需要注意的是越界问题，这里不可以判断 `num >2^31 - 1` 或者 `num < -2^31 `因为这么做`num`就已经越界了。所以需要用上面的式子判断。
顺带一提用Integer包装类的常量还是非常舒服的。省的去记int的范围。
然而实际跑了45ms，远远不如之前的字符串操作来得快，emmmmmm还是用字符串操作吧
###### 时间复杂度O(log(x))  空间复杂度O(1)


