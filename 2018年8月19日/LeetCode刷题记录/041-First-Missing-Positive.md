> Given an unsorted integer array, find the smallest missing positive integer.
#### Example：
> Input: [1,2,0]&emsp;&emsp;Output: 3
   Input: [3,4,-1,1]&emsp;Output: 2
   Input: [7,8,9,11,12]&emsp;Output: 1
#### Note：
>  Your algorithm should run in O(n) time and uses constant extra space.

#### 解释下题目：
> 就是找出数组中不存在的最小的正数，时间要求O(n)，排序就别想了


## 1. 找出最小的正数，然后判断这个正数是否合格
#### 实际耗时：10ms
```
public int firstMissingPositive(int[] nums) {
        int min = Integer.MAX_VALUE;
        Map map = new HashMap();
        int numOfNegative = 0;
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] <= 0) {
                numOfNegative++;
            } else if (nums[i] < min) {
                min = nums[i];
            }
            map.put(i, nums[i]);
        }
        if (numOfNegative == nums.length) {
            min = 1;
        }

        if (min == 1) {
            while (map.containsValue(min)) {
                min++;
            }
            return min;
        } else return 1;
    }
```
###### 踩过的坑：{-1}
&emsp;&emsp;思路就是简单的找出最小的正数，然后在找的时候顺手把所有的数放入HashMap中，方便之后查找。
&emsp;&emsp;然后是看最小的那个正数是不是1，如果不是1，那1肯定是缺失的正数。如果是1，那就需要接下去看2存不存在，3存不存在，直到找到最小的那个。
###### 时间复杂度O(2n) 首先是找最小的那个数用了O(n)，然后在不停找最小的那个数的时候遍历最坏情况是找到最后一个，因为HashMap是线性时间，所以也是O(n)，所以加起来是O(2n)
###### 空间复杂度O(1)
---------
## 1. 先让数组“有序”，然后查找
#### 实际耗时：10ms
```
public int firstMissingPositive2(int[] nums) {
        int i = 0;
        while (i < nums.length) {
            if (nums[i] == i + 1 || nums[i] <= 0 || nums[i] > nums.length) {
                i++;
            } else if (nums[nums[i] - 1] != nums[i]) {
                swap(nums, i, nums[i] - 1);
            } else {
                i++;
            }
        }
        i = 0;
        while (i < nums.length && nums[i] == i + 1) {
            i++;
        }
        return i + 1;
    }

    public void swap(int[] array, int i, int j) {
        int temp = array[i];
        array[i] = array[j];
        array[j] = temp;
    }
```
&emsp;&emsp;先把数组按照规定的规则进行处理：
&emsp;&emsp;举例：`int nums[] = {100, -1, 50, 2, 3, 5, 4, 6};`，经过处理之后：
|num|值|
|:--|:--|
|nums[0] | 100|
|nums[1] | 2|
|nums[2] | 3|
|nums[3] | 4|
|nums[4] | 5|
|nums[5] | 6|
|nums[6] | -1|
|nums[7] | 50|
简而言之就是如果有`num[i] == i+1` ,那么这个值必定在这个位置上。
结果与方法1相同。(HashMap真好用！)
###### 时间复杂度O(2n)
###### 空间复杂度O(1)
