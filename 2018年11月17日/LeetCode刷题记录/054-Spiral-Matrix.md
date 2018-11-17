> Given a matrix of *m* x *n* elements (m rows, n columns), return all elements of the matrix in spiral order.
#### Example：
> Input:
[
 [ 1, 2, 3 ],
 [ 4, 5, 6 ],
 [ 7, 8, 9 ]
]
Output: [1,2,3,6,9,8,7,4,5]

>Input:
[
  [1, 2, 3, 4],
  [5, 6, 7, 8],
  [9,10,11,12]
]
Output: [1,2,3,4,8,12,11,10,9,5,6,7]

#### 解释下题目：
> 就是从左上角开始螺旋打印这个数组


## 1. 按照要求螺旋打印呗，还能怎么样
#### 实际耗时：2ms
```
public List<Integer> spiralOrder(int[][] matrix) {
        List<Integer> list = new ArrayList<>();
        int row = matrix.length;
        if (0 == row) {
            //空数组就返回空
            return list;
        }
        int column = matrix[0].length;
        int count = row * column;

        //初始化四个顶点
        int row1 = 0;
        int column1 = 0;

        int row2 = 0;
        int column2 = column;

        int row3 = row;
        int column3 = column;

        int row4 = row;
        int column4 = 0;
        while (count > 0) {
            //第一趟从左往右
            for (int j = column1; j < column2; j++) {
                //System.out.print("1:");
                //System.out.println(matrix[row1][j]);
                list.add(matrix[row1][j]);
                count--;
            }
            if (0 == count) break;

            //第二趟从上往下
            for (int i = row2 + 1; i < row3; i++) {
                //System.out.print("2:");
                //System.out.println(matrix[i][column2 - 1]);
                list.add(matrix[i][column2 - 1]);
                count--;
            }
            if (0 == count) break;

            //第三趟从右到左
            for (int j = column3 - 2; j >= column4; j--) {
                //System.out.print("3:");
                //System.out.println(matrix[row3 - 1][j]);
                list.add(matrix[row3 - 1][j]);
                count--;

            }
            if (0 == count) break;

            //第四趟从下到上
            for (int i = row4 - 2; i > row1; i--) {
                //System.out.print("4:");
                //System.out.println(matrix[i][column4]);
                list.add(matrix[i][column4]);
                count--;

            }
            if (0 == count) break;

            //移动四个顶点
            row1++;
            column1++;

            row2++;
            column2--;

            row3--;
            column3--;

            row4--;
            column4++;
        }
        return list;
    }
```
###### 踩过的坑：[] 空的我又忘记处理了
&emsp;&emsp;思路：就是按照它说的照着打印就行了。处理的时候我是用了八个数用来记录四个顶点的位置，其实可以只用4个数，分别用来记录行的起始位置和列的起始位置即可。还有我是当判断所有数字都打印过了就退出，其实也可以判断列的起始位置重合且行的起始位置重合，说明输出完毕，都无伤大雅。
###### 时间复杂度O(nm) 不可能有更优的时间解了
###### 空间复杂度O(1)
---------


