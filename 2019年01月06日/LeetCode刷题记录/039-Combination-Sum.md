> Given a set of candidate numbers (`candidates`) (without duplicates) and a target number (`target`), find all unique combinations in `candidates` where the candidate numbers sums to `target`.

The same repeated number may be chosen from `candidates` unlimited number of times.
#### Example：
> Input: candidates = [2,3,6,7], target = 7,
A solution set is:
[
  [7],
  [2,2,3]
]

> Input: candidates = [2,3,5], target = 8,
A solution set is:
[
  [2,2,2,2],
  [2,3,3],
  [3,5]
]
#### Note：
> * All numbers (including target) will be positive integers.
> * The solution set must not contain duplicate combinations.

#### 解释下题目：
> 找出数组中可以使数字相加等于所给`target`的组合，同一个数字可以多次使用


## 1. 递归查找
#### 实际耗时：14ms
```
public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> lists = new ArrayList<>();
        getResult(lists, new ArrayList<>(), target, candidates, 0);
        return lists;
    }

    private void getResult(List<List<Integer>> lists, List<Integer> item, int target, int[] candidates, int start) {
        //小于0说明无解了
        if (target < 0) {
            return;
        }
        //找到一组解
        if (target == 0) {
            lists.add(new ArrayList<>(item));
            return;
        }
        //递归查找解
        for (int i = start; i < candidates.length; i++) {
            //查找优化，因为相同的其实没必要，只需要一个就可以了
            if (i > 0 && candidates[i] == candidates[i - 1]) {
                continue;
            }
            item.add(candidates[i]);
            getResult(lists, item, target - candidates[i], candidates, i);
            item.remove(item.size() - 1);
        }
    }
```
&emsp;&emsp;思路：我不需要知道有哪些组合可以相加得到目标数字，我只需要知道，当我把目标数字，即`target`，减去数组中的每个数字的时候，结果有没有在数组中，要是有的话就是找到了一组解。要是小于0说明当前这个组合无解。要是大于0且没有相同的数字，那么继续这么做。
###### 时间复杂度O(n^n) 极其恐怖的复杂度
###### 空间复杂度O(1)
---------
