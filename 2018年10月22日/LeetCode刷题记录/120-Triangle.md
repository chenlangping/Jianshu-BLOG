> Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.

#### Example：
> [
     [2],
    [3,4],
   [6,5,7],
  [4,1,8,3]
]
The minimum path sum from top to bottom is `11` (i.e., **2** + **3** + **5** + **1** = **11**).
#### Note：
> Bonus point if you are able to do this using only O(n) extra space, where n is the total number of rows in the triangle.

#### 解释下题目：
> 求解最短路径


## 1. 动态规划
#### 实际耗时：12ms
```
public int minimumTotal(List<List<Integer>> triangle) {
        int len = triangle.size();
        if (len <= 0) {
            return 0;
        }
        if (1 == len) {
            return triangle.get(0).get(0);
        }
        //num用来记录中间的答案
        int[] num = new int[len - 1];
        //i控制在第几行，j控制在第几列，从最后第二行开始
        for (int i = len - 2; i >= 0; i--) {
            for (int j = 0; j < i + 1; j++) {
                //left记录左下方的数字(emmm其实应该是正下方)
                int left = triangle.get(i + 1).get(j);
                //right记录右下方的数字
                int right = triangle.get(i + 1).get(j + 1);
                //self是自己
                int self = triangle.get(i).get(j);
                //num用来记录结果
                num[j] = Math.min(left + self, right + self);
                //把新的结果设置上
                triangle.get(i).set(j, num[j]);
            }
        }
        return num[0];
    }
```
&emsp;&emsp;思路就是从倒数第二行，把每个数字都和它左下方和右下方两个相加，得到较小的那个值赋给它，这样就相当于求得了从它到最底下的最短路径。然后依次照做即可。
###### 时间复杂度O(n^2)     n为List的“行数”
###### 空间复杂度O(n)        n为List的“行数”
---------
