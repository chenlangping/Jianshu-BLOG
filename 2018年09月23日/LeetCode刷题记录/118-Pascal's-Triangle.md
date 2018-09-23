> Given a non-negative integer *numRows*, generate the first *numRows* of Pascal's triangle.

![Pascal三角形示例图](http://upload-images.jianshu.io/upload_images/13050335-935d88f2e6b66a01.gif?imageMogr2/auto-orient/strip)
#### Example：
> Input: 5
Output:
[
     [1],
    [1,1],
   [1,2,1],
  [1,3,3,1],
 [1,4,6,4,1]
]

#### 解释下题目：
> 就是生成一个杨辉三角（Pascal三角）


## 1. 根据需要求解呗
#### 实际耗时：2ms
```
public List<List<Integer>> generate(int numRows) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> list = new ArrayList<>();
        for (int i = 0; i < numRows; i++) {
            //第一位一定是1
            list.add(0, 1);
            for (int j = 1; j < list.size() - 1; j++) {
                list.set(j, list.get(j) + list.get(j + 1));
            }
            result.add(new ArrayList<>(list));
        }
        return result;
    }
```
&emsp;&emsp;思路就是按照杨辉三角的思路做呗，每个元素等于它“肩上”的两个数字相加。
###### 时间复杂度O(n^2)
###### 空间复杂度O(n^2)
---------
