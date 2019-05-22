> Given a 2D binary matrix filled with 0's and 1's, find the largest rectangle containing only 1's and return its area.
#### Example：
> Input:
[
  ["1","0","1","0","0"],
  ["1","0","1","1","1"],
  ["1","1","1","1","1"],
  ["1","0","0","1","0"]
]
Output: 6

#### 解释下题目：
> 找出二维矩阵中最大的矩阵，1代表有这个点，0代表没这个点。


## 1. 借鉴084的方法
#### 实际耗时：34ms
```
public int maximalRectangle(char[][] matrix) {
        if (matrix == null || matrix.length == 0 || matrix[0].length == 0)
            return 0;
        int cLen = matrix[0].length;
        int rLen = matrix.length;
        int[] h = new int[cLen + 1];
        h[cLen] = 0;
        int max = 0;


        for (int row = 0; row < rLen; row++) {
            Stack<Integer> s = new Stack<>();
            for (int i = 0; i < cLen + 1; i++) {
                if (i < cLen) {
                    if (matrix[row][i] == '1') {
                        h[i] += 1;
                    } else {
                        h[i] = 0;
                    }
                }
                //System.out.println(Arrays.toString(h));
                if (s.isEmpty() || h[s.peek()] <= h[i])
                    s.push(i);
                else {
                    while (!s.isEmpty() && h[i] < h[s.peek()]) {
                        int top = s.pop();
                        int height = h[top];
                        int left;
                        if (s.isEmpty()) {
                            left = 0;
                        } else {
                            left = s.peek() + 1;
                        }
                        int right = i;
                        int area = height * (right - left);
                        if (area > max)
                            max = area;
                    }
                    s.push(i);
                }
            }
        }
        return max;
    }
```
&emsp;&emsp;思路如果理解了[084](https://www.jianshu.com/p/4e988a6bf705)就很简单，只需要理解如果height遇到了0，那就直接置0。理解这一点就可以了。
###### 时间复杂度O(n^2)
###### 空间复杂度O(n)
---------
