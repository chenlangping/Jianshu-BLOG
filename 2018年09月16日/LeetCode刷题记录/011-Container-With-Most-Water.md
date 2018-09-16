> Given *n* non-negative integers `a1`, `a2`, ..., `an` , where each represents a point at coordinate (`i`, `ai`). *n* vertical lines are drawn such that the two endpoints of line `i` is at (`i`, `ai`) and (`i`, `0`). Find two lines, which together with x-axis forms a container, such that the container contains the most water.
#### Example：
> Input: [1,8,6,2,5,4,8,3,7]
Output: 49

![示例图](http://upload-images.jianshu.io/upload_images/13050335-e11da1054a7d5f84.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
#### Note：
> You may not slant the container and *n* is at least 2

#### 解释下题目：
> 找到两根之间能接水最多的柱子（忽略中间的柱子）


## 1. 暴力破解
#### 实际耗时：266ms
```
public int maxArea(int[] height) {
        int max = 0;    //max为最后返回的最大面积
        for (int i = 0; i < height.length - 1; i++) {
            for (int j = i + 1; j < height.length; j++) {
                int small = height[i] > height[j] ? height[j] : height[i];    //small是数组中值较少的那个
                int square = Math.abs(small * (j - i));
                if (square > max) {
                    max = square;
                }
            }
        }
        return max;
    }
```
&emsp;&emsp;暴力破解要什么思路，直接撸起袖子上呗
###### 时间复杂度O(n^2)
###### 空间复杂度O(1)
---------
## 2. 利用两个"指针"
#### 实际耗时：5ms
```
public int maxArea2(int[] height) {
        int max = 0;
        int start = 0;
        int end = height.length - 1;
        while (start < end) {
            int wid = end - start;
            if (height[start] < height[end]) {
                max = Math.max(max, wid * height[start]);
                start++;
            } else {
                max = Math.max(max, wid * height[end]);
                end--;
            }
        }
        return max;
    }
```
&emsp;&emsp;思路：想要矩形的面积最大，那就是最长的长和最宽的宽组合在一起。首先从最宽开始找，最宽毫无疑问就是数组的长度-1。然后将这个宽与数组首尾中较长的数相乘，就是目前最大的区域。然后如果要得到比这个更大的解，那么宽一定会缩小，也就是说一定要长比之前的长才行。思路就是将长比较短的那一端开始往中间缩，只有这样才有可能得到最优解。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
