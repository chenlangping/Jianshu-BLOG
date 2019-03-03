> Find the **k** th largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.
#### Example：
> Input: [3,2,1,5,6,4] and k = 2
Output: 5

> Input: [3,2,3,1,2,4,5,5,6] and k = 4
Output: 4
#### Note：
> You may assume k is always valid, 1 ≤ k ≤ array's length.

#### 解释下题目：
> 找数组中第k大的元素，返回其数值


## 1. 分而治之
#### 实际耗时：3ms
```
public int findKthLargest(int[] nums, int k) {
        return findKthLargest(nums, k, nums.length);
    }

    private int findKthLargest(int[] nums, int k, int length) {
        if (length == 1) {
            return nums[0];
        }
        int i;
        int j = 0;
        int l = 0;
        int[] small = new int[length];
        int[] big = new int[length];
        //基准使用随机数来确定
        int pivot = (int) ((length - 1) * Math.random());
        for (i = 0; i < length; i++) {
            //需要排除掉下标为pivot的本身，注意！！仅仅是排除自己，和自己值一样的是不会影响结果的！！
            if (i == pivot) {
                continue;
            }
            //这里格外注意，其实把和nums[pivot]值相等的数归并到哪一边都无所谓
            if (nums[i] < nums[pivot]) {
                small[j++] = nums[i];
            } else {
                big[l++] = nums[i];
            }
        }
        if (k == l + 1) {
            return nums[pivot];
        } else if (k > l + 1) {
            //在pivot左边找
            return findKthLargest(small, k - l - 1, j);
        } else {
            //在pivot右边找
            return findKthLargest(big, k, l);
        }
    }
```
&emsp;&emsp;思路就是如果我想找第K个数，那我先随便找一个数，把比它大的放一边，把比它小的放一边。（这里特别注意，值和它相同的可以放大的一边也可以放小的一边，但是**它自己**是必须单独拎出来的），之后可以我们设比它小的有j个，比它大的有l个，那么根据k和l的关系，就可以决定出应该去哪边继续找。而且还不需要conquer。
###### 时间复杂度O(n) 取决于Pivot的选取，由于是随机的，所以可以证明是O(n)
###### 空间复杂度O(1)
---------

## 2. 快排解决之
#### 实际耗时：5ms
```
public int findKthLargest2(int[] nums, int k) {
        Arrays.sort(nums);
        return nums[nums.length - k];
    }
```
&emsp;&emsp;排序之后什么都解决了
###### 时间复杂度O(nlogn)
###### 空间复杂度O(1)
---------
