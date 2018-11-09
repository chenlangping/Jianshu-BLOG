> Given a collection of candidate numbers (`candidates`) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.
Each number in `candidates` may only be used once in the combination.
#### Example：
> Input: candidates = [10,1,2,7,6,1,5], target = 8,
A solution set is:
[
  [1, 7],
  [1, 2, 5],
  [2, 6],
  [1, 1, 6]
]

> Input: candidates = [2,5,2,1,2], target = 5,
A solution set is:
[
  [1,2,2],
  [5]
]
#### Note：
> All numbers (including `target`) will be positive integers.
The solution set must not contain duplicate combinations.

#### 解释下题目：
> 就是找出所有能加起来等于`target`的组合，不可以有重复


## 1. 递归
#### 实际耗时：18ms
```
public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> lists = new ArrayList<>();
        Arrays.sort(candidates);
        List<Integer> list = new ArrayList<>();
        getResult(lists, list, candidates, target, -1);
        return lists;
    }

    public void getResult(List<List<Integer>> lists, List list, int[] candidates, int target, int start) {
        if (target < 0) {
            return;
        } else if (0 == target) {
            if (!lists.contains(new ArrayList<>(list))) {
                lists.add(new ArrayList<>(list));
                return;
            }
            return;
        } else {
            for (int i = start + 1; i < candidates.length; i++) {
                if (i > start + 1 && candidates[i] == candidates[i - 1]) {
                    //去重操作
                    continue;
                }
                list.add(candidates[i]);
                getResult(lists, list, candidates, target - candidates[i], i);
                list.remove(list.size() - 1);
            }
        }
    }
```
###### 踩过的坑：{2,2,2} target = 2
&emsp;&emsp;思路和之前的[039](https://www.jianshu.com/p/f0f65d93a4f4)一样，只是自己不能多次使用了，那么就进行去重操作呗。
###### 时间复杂度O(2^n)  也很复杂了
###### 空间复杂度O(1)
---------
