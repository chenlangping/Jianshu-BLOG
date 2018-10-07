> Given *n* non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.
![Above is a histogram where width of each bar is 1, given height = [2,1,5,6,2,3].](http://upload-images.jianshu.io/upload_images/13050335-fa63c781cb3396e9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![The largest rectangle is shown in the shaded area, which has area = 10 unit.](http://upload-images.jianshu.io/upload_images/13050335-30855383b03543d8.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### Example：
> Input: [2,1,5,6,2,3]
Output: 10

#### 解释下题目：
> 给定一个数组，找出其中所能包含最大的矩形的面积


## 1. 遍历求解
#### 实际耗时：500ms
```
public int largestRectangleArea(int[] heights) {
        if (heights.length == 0 || heights == null) {
            return 0;
        }
        int maxArea = heights[0];
        int[] left = new int[heights.length];
        int[] right = new int[heights.length];
        //0左边的是-1
        left[0] = -1;
        //最右边的那根柱子的右边就是数组的长度
        left[heights.length - 1] = heights.length;
        //为每一个柱子找到它左边的离它最近的比它短的柱子
        for (int i = 1; i < heights.length; i++) {
            int j = i - 1;
            while (j >= 0 && heights[j] >= heights[i]) {
                j--;
            }
            left[i] = j;
        }
        //为每一个柱子找到它右边的离它最近的比它短的柱子
        for (int i = heights.length - 1; i >= 0; i--) {
            int j = i + 1;
            while (j < heights.length && heights[j] >= heights[i]) {
                j++;
            }
            right[i] = j;
        }
        //此时已经算出来每条柱子最左边和最右边的了
        //System.out.println(Arrays.toString(left));
        //System.out.println(Arrays.toString(right));

        //计算面积
        for (int i = 0; i < heights.length; i++) {
            maxArea = Math.max(maxArea, heights[i] * (right[i] - left[i] - 1));
        }
        return maxArea;
    }
```
###### 踩过的坑：[0,9] 和 [2,3]
&emsp;&emsp;思路：从一开始我想的是把这个分解成子问题求解，也就是我想知道一个数组最大的面积，我首先需要知道这个数组删去最后一个元素的最大面积，然后最后把这个删去的元素加入进行判断。后来发现有点复杂，之后从[接雨水](https://www.jianshu.com/p/d66944915365)这道题目获得启示，最大的面积中，必定是包含一条柱子的，那么我针对每一条柱子，只需要求出包含这个柱子所能得到的最大解，然后从`nums.length`个解中找出最大的即可。
###### 时间复杂度O(n^2)
###### 空间复杂度O(2n)
---------
