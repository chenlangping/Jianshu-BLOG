> Given a collection of numbers that might contain duplicates, return all possible unique permutations.
#### Example：
> Input: [1,1,2]
Output:
[
  [1,1,2],
  [1,2,1],
  [2,1,1]
]

#### 解释下题目：
> 求全排列


## 1. 递归+数组
#### 实际耗时：6ms
```
public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        boolean used[] = new boolean[nums.length];
        Arrays.sort(nums);
        recursion(result, new ArrayList<>(), nums, used);
        return result;
    }

    private void recursion(List<List<Integer>> result, List<Integer> tempList,
                           int[] nums, boolean[] used) {
        if (tempList.size() == nums.length) {
            result.add(new ArrayList<>(tempList));
            return;
        }

        for (int i = 0; i < nums.length; i++) {
            //如果目前的数字已经被用过了，直接下一个数字
            if (used[i]) {
                continue;
            }
            //如果目前的这个数字和之前的一样，且前一个数字没被用过，那么不需要了
            if (i >= 1 && nums[i - 1] == nums[i] && !used[i - 1]) {
                continue;
            }
            used[i] = true;
            tempList.add(nums[i]);
            recursion(result, tempList, nums, used);
            used[i] = false;
            tempList.remove(tempList.size() - 1);
        }


    }
```

&emsp;&emsp;思路:我一开始是没有`if (i >= 1 && nums[i - 1] == nums[i] && !used[i - 1])`这个条件来限制，而是直接在加入`result`的时候进行判断：`if (tempList.size() == nums.length && !result.contains(tempList))`，这么做当然正确性是没问题的，但是极度耗时，因为每次加入的时候都需要去遍历整个`list`，导致极其耗时。
###### 时间复杂度O(n! * n)
###### 空间复杂度O(全排列个数)
---------
