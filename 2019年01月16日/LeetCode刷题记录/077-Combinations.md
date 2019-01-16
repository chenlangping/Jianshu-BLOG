> Given two integers *n* and *k*, return all possible combinations of *k* numbers out of 1 ... n.
#### Example：
> Input: n = 4, k = 2
Output:
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]

#### 解释下题目：
> 说白了就是加了点条件的全排列罢了。


## 1. 方法1
#### 实际耗时：51ms
```
public static List<List<Integer>> combine(int n, int k) {
        List<List<Integer>> combs = new ArrayList<>();
        combine(combs, new ArrayList<Integer>(), 1, n, k);
        return combs;
    }

    public static void combine(List<List<Integer>> combs, List<Integer> comb, int start, int n, int k) {
        if (k == 0) {
            combs.add(new ArrayList<Integer>(comb));
            return;
        }
        for (int i = start; i <= n; i++) {
            comb.add(i);
            combine(combs, comb, i + 1, n, k - 1);
            comb.remove(comb.size() - 1);
        }
    }

```
&emsp;&emsp;思路很简单。就是全排列的思路。深度优先的算法
###### 时间复杂度O(n*(n-1)*(n-2)......(n-k))
###### 空间复杂度O(n*(n-1)*(n-2)......(n-k))
---------
