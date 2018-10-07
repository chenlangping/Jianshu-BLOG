> Given two sorted integer arrays *nums1* and *nums*2, merge *nums2* into *nums1* as one sorted array.
#### Example：
> Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3
Output: [1,2,2,3,5,6]
#### Note：
> * The number of elements initialized in *nums1* and *nums2* are *m* and *n* respectively.
> * You may assume that *nums1* has enough space (size that is greater or equal to *m* + *n*) to hold additional elements from *nums2*.

#### 解释下题目：
> 把两个有序的数组合并成一个有序的数组


## 1. 从后向前判断
#### 实际耗时：3ms
```
public void merge(int[] nums1, int m, int[] nums2, int n) {
        //i指向最后的nums1的最后一个真实的数，j指向nums2的最后一个数，k指向nums1的末尾
        int i = m - 1;
        int j = n - 1;
        int k = m + n - 1;
        while (i >= 0 && j >= 0) {
            //如果nums1比较大，那么就放入nums1的数据
            if (nums1[i] > nums2[j]) {
                nums1[k] = nums1[i];
                k--;
                i--;
            } else {
                //否则放入nums2的数据
                nums1[k] = nums2[j];
                k--;
                j--;
            }
        }
        //最后如果nums1有剩余，那么不用管；如果是num2有剩余，那么依次放入即可
        while (j >= 0) {
            nums1[k] = nums2[j];
            k--;
            j--;
        }
    }
```
&emsp;&emsp;思路：没什么难的，只是需要想清楚记得从后往前做，否则会覆盖一些仍然没判断过的，造成麻烦。
###### 时间复杂度O(n+m)
###### 空间复杂度O(1)
---------
