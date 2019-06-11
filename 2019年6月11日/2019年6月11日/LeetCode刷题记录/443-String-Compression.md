> Given an array of characters, compress it [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm).
The length after compression must always be smaller than or equal to the original array.
Every element of the array should be a **character** (not int) of length 1.
After you are done **modifying the input array [in-place](https://en.wikipedia.org/wiki/In-place_algorithm)**, return the new length of the array.

#### Example：
> Input:
["a","a","b","b","c","c","c"]
Output:
Return 6, and the first 6 characters of the input array should be: ["a","2","b","2","c","3"]
Explanation:
"aa" is replaced by "a2". "bb" is replaced by "b2". "ccc" is replaced by "c3".
#### Note：
> Could you solve it using only O(1) extra space?

#### 解释下题目：
> 压缩字符串


## 1. 在原先的数组上修改
#### 实际耗时：1ms
```java
public int compress(char[] chars) {
        int pos = 0;
        for (int i = 0; i < chars.length; i++) {
            char tmp = chars[i];
            int count = 0;
            int cur = i;
            while (cur < chars.length) {
                if (tmp == chars[cur]) {
                    count++;
                    cur++;
                } else {
                    break;
                }
            }

            if (1 == count) {
                chars[pos] = tmp;
                pos++;
            } else {
                chars[pos] = tmp;
                pos++;
                char[] num = String.valueOf(count).toCharArray();
                for (char a : num) {
                    chars[pos] = a;
                    pos++;
                }
            }

            i = cur - 1;
        }
        return pos;
    }
```
&emsp;&emsp;其实这道题目的“难点”我觉得在于，如果给定的数字的，比如1003，怎么把它写成四个字符的方法，我用的是先转成String然后再转成char[]数组。同样的python也是先转成了str然后再从str中提取。最后是在python的时候，学到了从str到List仅仅需要list(str)即可，我以前一直以为是str.split("")....因为从list到str是join.
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
