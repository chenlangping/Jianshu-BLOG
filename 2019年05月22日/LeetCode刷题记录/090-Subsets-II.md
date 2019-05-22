> Given a collection of integers that might contain duplicates, **nums**, return all possible subsets (the power set).
#### Example：
> Input: [1,2,2]
Output:
[
  [2],
  [1],
  [1,2,2],
  [2,2],
  [1,2],
  []
]
#### Note：
> Note: The solution set must not contain duplicate subsets.

#### 解释下题目：
> 输出所有的不重复子集


## 1. 在[078](https://www.jianshu.com/p/a0116c05c0c9)的基础上修改呗
#### 实际耗时：44ms
```
public List<List<Integer>> subsetsWithDup(int[] nums) {
        List<List<Integer>> result = new ArrayList<>();
        List<Integer> temp = new ArrayList<>();
        Arrays.sort(nums);
        recursion(result, temp, 0, nums);
        return result;
    }

    public void recursion(List<List<Integer>> result, List<Integer> temp, int start, int[] nums) {
        if (!result.contains(new ArrayList<>(temp))) {
            result.add(new ArrayList<>(temp));
        }
        for (int i = start; i < nums.length; i++) {
            temp.add(nums[i]);
            recursion(result, temp, i + 1, nums);
            temp.remove(temp.size() - 1);
        }
    }
```
###### 踩过的坑：`{4,4,4,1,4}` 主要是忘了对数组进行排序
&emsp;&emsp;思路与[078](https://www.jianshu.com/p/a0116c05c0c9)一样，只是加了重复判断
###### 时间复杂度O(2^n)
###### 空间复杂度O(1)
---------
## 2. 在1之上进行修改
#### 实际耗时：3ms
```
public void recursion2(List<List<Integer>> result, List<Integer> temp, int start, int[] nums) {
        if (start <= nums.length) {
            result.add(temp);
        }
        int i = start;
        while (i < nums.length) {
            temp.add(nums[i]);
            recursion2(result, new ArrayList<>(temp), i + 1, nums);
            temp.remove(temp.size() - 1);
            i++;
            while (i < nums.length && nums[i] == nums[i - 1]) {
                i++;
            }
        }
        return;
    }
```
&emsp;&emsp;思路其实就是加了一个如果下一个数字和当前的数字一样，就直接跳过。
###### 时间复杂度O(2^n)
###### 空间复杂度O(1)
---------
