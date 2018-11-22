> Implement **next permutation**, which rearranges numbers into the lexicographically next greater permutation of numbers.
If such arrangement is not possible, it must rearrange it as the lowest possible order (ie, sorted in ascending order).
The replacement must be **[in-place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.
Here are some examples. Inputs are in the left-hand column and its corresponding outputs are in the right-hand column.
`1,2,3` → `1,3,2`
`3,2,1` → `1,2,3`
`1,1,5` → `1,5,1`


#### 解释下题目：
> 就是C++的Next Permutation实现。


## 1. 我是真的没想到这个方法
#### 实际耗时：20ms
```
public void nextPermutation(int[] nums) {
        //i用来记录从右边开始的第一个递减的下标
        int i = nums.length - 2;
        while (i >= 0 && nums[i + 1] <= nums[i]) {
            i--;
        }
        if (i >= 0) {
            //i=-1的时候说明整个数组都是降序的
            int j = nums.length - 1;
            while (j >= 0 && nums[j] <= nums[i]) {
                j--;
            }
            System.out.println("j= " + j);
            swap(nums, i, j);
        }
        reverse(nums, i + 1);
    }

    /**
     * 将从start开始的数组，按从小到大的顺序排列
     *
     * @param nums  目标数组
     * @param start 开始位置
     */
    private void reverse(int[] nums, int start) {
        int i = start, j = nums.length - 1;
        while (i < j) {
            swap(nums, i, j);
            i++;
            j--;
        }
    }

    /**
     * 交换数组指定位置的数
     *
     * @param nums
     * @param i
     * @param j
     */
    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
```
&emsp;&emsp;思路就是下面那张图，说实话这道题一开始我想的是把所有的都打印一遍，然后照着查就行了。但是时间效率是O(n!)，显然不符合要求。
![思路动态图](http://upload-images.jianshu.io/upload_images/13050335-ef412215a9e7052c.gif?imageMogr2/auto-orient/strip)
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------
