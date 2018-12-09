> Given a set of **distinct** integers, *nums*, return all possible subsets (the power set).
#### Example：
> Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
#### Note：
> The solution set must not contain duplicate subsets.

#### 解释下题目：
> 输出一个数组(集合)的所有子集


## 1. 递归
#### 实际耗时：1ms
```
public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        recursion(result, temp, 0, nums);
        return result;
    }

    public void recursion(List<List<Integer>> result, List<Integer> temp, int start, int[] nums) {
        result.add(new ArrayList<>(temp));
        for (int i = start; i < nums.length; i++) {
            temp.add(nums[i]);
            recursion(result, temp, i + 1, nums);
            temp.remove(temp.size() - 1);
        }
    }
```
&emsp;&emsp;思路很：简单的递归，从空集开始，每次都先记录下自己，然后再加入另外一个元素，递归进去，然后再把这个元素删除，开始下一个循环。
###### 时间复杂度O(2^n) 非常复杂
###### 空间复杂度O(1)
---------
