> Given a non-negative index *k* where *k* ≤ 33, return the *k*th index row of the Pascal's triangle.

![示意图](http://upload-images.jianshu.io/upload_images/13050335-0778431f61d5ce7d.gif?imageMogr2/auto-orient/strip)
#### Example：
> Input: 3
Output: [1,3,3,1]
#### Note：
> Note that the row index starts from 0.

#### 解释下题目：
> 输出杨辉三角的某一行，注意行是从0开始的


## 1. 跟[118](https://www.jianshu.com/p/82e0ec225642)一样的解法呗
#### 实际耗时：1ms
```
public List<Integer> getRow(int rowIndex) {
        List<Integer> list = new ArrayList<>();
        list.add(0, 1);
        for (int i = 0; i < rowIndex; i++) {
            //第一位一定是1
            list.add(0, 1);
            for (int j = 1; j < list.size() - 1; j++) {
                list.set(j, list.get(j) + list.get(j + 1));
            }
        }
        return list;
    }
```
&emsp;&emsp;思路是真的没什么好写的......
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
