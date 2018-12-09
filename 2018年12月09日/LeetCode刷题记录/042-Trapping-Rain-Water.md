> Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.


#### Example：
> Input: [0,1,0,2,1,0,1,3,2,1,2,1]
   Output: 6

#### 解释下题目：
> 就是给一个数组，这个数组对应矩形的条(图中的黑色)，然后看这些条组成的能装多少“雨水”，图中的蓝色部分。


## 1. 自己的骚操作
#### 实际耗时：没通过
```
public int trap(int[] height) {
        List<Integer> list = new ArrayList<>();
        int start = 0;
        while (-1 != (start = findHighest(height, start))) {
            list.add(start);
            start++;
        }


        System.out.println(list);
        int[] array = new int[list.size()];
        for (int i = 0; i < list.size(); i++) {
            array[i] = list.get(i);
        }
        int result = 0;
        int left = 0;
        int right = 0;
        for (int i = 0; i < array.length - 1; i++) {
            if (array[i] + 1 == array[i + 1]) {
                left = i;
                continue;
            } else {
                //计算
                right = i + 1;
                result += calculateTwoColumn(height, array[left], array[right]);
            }
        }
        return result;
    }

    public int findHighest(int[] height, int start) {
        while (height.length > start + 1) {
            if (height[start + 1] >= height[start]) {
                start++;
            } else {
                return start;
            }
        }
        return -1;
    }

    public int calculateTwoColumn(int[] height, int start, int end) {
        int wid = end - start - 1;
        int high = Math.min(height[start], height[end]);
        int exist = 0;
        for (int i = start + 1; i < end; i++) {
            exist += height[i];
        }
        return wid * high - exist;
    }

```
&emsp;&emsp;我之前的思路是，找到局部最高的那些条，然后计算即可。后来发现就算是局部最长的条，也可能被其他的更长的条给包围，所以出现了错误。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
## 2. 暴力破解
#### 实际耗时：97ms
代码比较简单就黏贴过来了
```
public int trap2(int[] height) {
        int result = 0;
        int size = height.length;
        for (int i = 1; i < size - 1; i++) {
            int max_left = 0, max_right = 0;
            for (int j = i; j >= 0; j--) { //Search the left part for max bar size
                max_left = Math.max(max_left, height[j]);
            }
            for (int j = i; j < size; j++) { //Search the right part for max bar size
                max_right = Math.max(max_right, height[j]);
            }
            result += Math.min(max_left, max_right) - height[i];
        }
        return result;
    }
```

&emsp;&emsp;思路很简单，针对每一个柱，如果它上面能放雨水，那么必然有它的左边和它的右边有比它还高的柱子，找到左右两边最高(可能是自己)的，然后找出其中比较矮的那根，减去它自己就是它所能存放的雨水。
###### 时间复杂度O(n^2)
###### 空间复杂度O(1)
---------
## 3. 使用动态规划
#### 实际耗时：13ms
```
public int trap3(int[] height) {
        int len = height.length;
        if (0 == len) {
            return 0;
        }
        int[] left = new int[len];
        int[] right = new int[len];
        left[0] = height[0];
        right[len - 1] = height[len - 1];
        int left_max = height[0];
        int right_max = height[len - 1];
        int result = 0;

        //按照从左到右递增的顺序填充数组，并记录填充的大小
        for (int i = 1; i < len; i++) {
            if (height[i] <= left_max) {
                left[i] = left_max;
            } else {
                left_max = height[i];
                left[i] = left_max;
            }
        }
        //按照从右到左递增的顺序填充数组，并记录填充的大小
        for (int i = len - 2; i >= 0; i--) {
            if (height[i] <= right_max) {
                right[i] = right_max;
            } else {
                right_max = height[i];
                right[i] = right_max;
            }
        }

        //计算重叠部分
        for (int i = 0; i < len; i++) {
            result += Math.min(left[i] - height[i], right[i] - height[i]);
        }

        return result;
    }
```
&emsp;&emsp;按照从左到右递增的方式“补全”数组，然后按照从右到左的方式“补全”数组，这两个重叠的部分就是所求的解。
###### 时间复杂度O(n)
###### 空间复杂度O(3)
---------
