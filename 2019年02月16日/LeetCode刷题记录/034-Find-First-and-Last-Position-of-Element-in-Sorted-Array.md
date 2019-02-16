> Given an array of integers `nums` sorted in ascending order, find the starting and ending position of a given `target` value.
Your algorithm's runtime complexity must be in the order of O(*log n*).
If the target is not found in the array, return `[-1, -1]`.
#### Example：
> Input: nums = [5,7,7,8,8,10], target = 8
Output: [3,4]

> Input: nums = [5,7,7,8,8,10], target = 6
Output: [-1,-1]

#### 解释下题目：
> 给定一个有序的数组，给定一个数字，找出这个数字在这个有序数组中第一次出现和最后一次出现的位置


## 1. 二分法查找
#### 实际耗时：4ms
```
public int[] searchRange(int[] nums, int target) {
        int mid = binarySearch(nums, target);
        int[] result = {-1, -1};
        if (-1 == mid) {
            return result;
        } else {
            int left = mid;
            int right = mid;
            result[0] = left;
            result[1] = right;
            while (left >= 1) {
                if (nums[left] == nums[left - 1]) {
                    left--;
                } else {
                    result[0] = left;
                    break;
                }
                //这句至关重要
                result[0] = left;
            }
            while (right < nums.length - 1) {
                if (nums[right] == nums[right + 1]) {
                    right++;
                } else {
                    result[1] = right;
                    break;
                }
                //这句至关重要
                result[1] = right;
            }
            return result;
        }

    }

    /**
     * 二分查找法
     *
     * @param nums   待查找的数组
     * @param target 目标数字
     * @return 对应的下标，不存在返回-1
     */
    public static int binarySearch(int[] nums, int target) {
        int small = 0;
        int big = nums.length - 1;
        int mid;
        int result = -1;
        while (big >= small) {
            mid = (big + small) >> 1;
            if (nums[mid] == target) {
                result = mid;
                return result;
            } else if (nums[mid] > target) {
                big = mid - 1;
            } else {
                small = mid + 1;
            }
        }
        return result;
    }
```
###### 踩过的坑：{1,1}  target = 1 ; {2,2} target = 2
&emsp;&emsp;思路：首先使用二分法找到对应数字的下标，然后开始往前找和往后找。稍微注意下注释中的“至关重要”语句。
###### 时间复杂度O(log n)
###### 空间复杂度O(1)
---------
