> Given a collection of **distinct** integers, return all possible permutations.
#### Example：
> Input: [1,2,3]
Output:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]

#### 解释下题目：
> 输出全排列喽


## 1. 递归调用呗
#### 实际耗时：3ms
```
public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        recursion(result, new ArrayList<>(), nums);
        return result;
    }

    private void recursion(List<List<Integer>> list, List<Integer> tempList, int[] nums) {
        if (tempList.size() == nums.length) {
            list.add(new ArrayList<>(tempList));
        } else {
            for (int i = 0; i < nums.length; i++) {
                if (tempList.contains(nums[i])) {
                    //如果已经存在了，则不需要再遍历了
                    continue;
                }
                tempList.add(nums[i]);
                recursion(list, tempList, nums);
                tempList.remove(tempList.size() - 1);
            }
        }
    }
```
&emsp;&emsp;就是一个全排列，先把第一个加进来，然后不断加东西进来，如果长度等于数组的长度，说明找到一个解了，否则就继续深入，唯一值得注意的点就是需要判断这个数字有没有出现过而已。
###### 时间复杂度O(n!)
###### 空间复杂度O(n!)
---------
