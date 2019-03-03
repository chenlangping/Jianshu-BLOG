> Validate if a given string can be interpreted as a decimal number.
#### Example：
> "0" => true
" 0.1 " => true
"abc" => false
"1 a" => false
"2e10" => true
" -90e3   " => true
" 1e" => false
"e3" => false
" 6e-1" => true
" 99e2.5 " => false
"53.5e93" => true
" --6 " => false
"-+3" => false
"95a54e53" => false

#### 解释下题目：
> 判断一个数是不是合法的小数


## 1. 老老实实做
#### 实际耗时：42ms
```
public boolean isNumber(String s) {
        boolean hasE = false;
        int numOfPlus = 0;
        int numOfMinus = 0;
        boolean hasPoint = false;
        if (s.length() == 0) {
            return false;
        }
        s = s.trim();
        for (char c : s.toCharArray()) {
            //合法的只可能有 +-.e 0-9这几种，且+-.e只可能出现一次
            if (c == '+') {
                if (numOfPlus == 2) {
                    return false;
                }
                numOfPlus++;
            } else if (c == '-') {
                if (numOfMinus == 2) {
                    return false;
                }
                numOfMinus++;
            } else if (c == '.') {
                if (hasPoint) {
                    return false;
                }
                hasPoint = true;
            } else if (c == 'e') {
                if (hasE) {
                    return false;
                }
                hasE = true;
            } else if (Character.isDigit(c)) {
                continue;
            } else {
                return false;
            }
        }

        if (hasE) {
            //如果有e 则以e为中间点分割
            String[] res = s.split("e");
            if (res.length != 2) {
                return false;
            }
            String front = res[0];
            String back = res[1];

            if (isUnsignedDecimal(front) || isUnsignedInteger(front) || isSignedDecimal(front) || isSignedInteger(front)) {
                if (isSignedInteger(back) || isUnsignedInteger(back)) {
                    return true;
                } else {
                    return false;
                }
            } else {
                return false;
            }
        } else {
            //没有e
            if (isUnsignedDecimal(s) || isUnsignedInteger(s) || isSignedDecimal(s) || isSignedInteger(s)) {
                return true;
            } else {
                return false;
            }
        }
    }

    public boolean isUnsignedInteger(String s) {
        //判断是不是无符号的整数
        if (s.length() == 0 || s == null) {
            return false;
        }
        for (char c : s.toCharArray()) {
            if (!Character.isDigit(c)) {
                return false;
            }
        }
        return true;
    }

    public boolean isUnsignedDecimal(String s) {
        //判断是不是无符号的小数
        if (s.length() == 1 && s.charAt(0) == '.') {
            return false;
        }
        if (s.length() == 0 || s == null) {
            return false;
        }
        int flag = -1;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (c == '.') {
                if (flag >= 0) {
                    return false;
                } else {
                    flag = i;
                }
            } else if (!Character.isDigit(c)) {
                return false;
            }
        }
        return true;
    }

    public boolean isSignedInteger(String s) {
        if (s.length() == 0 || s == null) {
            return false;
        }
        if ((s.charAt(0) == '-' || s.charAt(0) == '+') && isUnsignedInteger(s.substring(1))) {
            return true;
        } else {
            return false;
        }
    }

    public boolean isSignedDecimal(String s) {
        if (s.length() == 0 || s == null) {
            return false;
        }
        if ((s.charAt(0) == '-' || s.charAt(0) == '+') && isUnsignedDecimal(s.substring(1))) {
            return true;
        } else {
            return false;
        }
    }
```
###### 踩过的坑：太多了，自己慢慢试吧
&emsp;&emsp;思路是这样的，首先对于这一整个字符串，如果有`e`，那么就说明可以分成两个子问题，左边的和右边，左边的可以是有/无符号的整数和小数，而右边的仅仅是能有/无符号的整数。然后就分别写了4个函数来对应这些判断。需要注意的是，类似`3.` 和`.1`都算小数，所以注意下
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
