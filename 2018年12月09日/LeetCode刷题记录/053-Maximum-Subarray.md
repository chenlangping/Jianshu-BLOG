> Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
#### Example：
> Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
#### Note：
> If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

#### 解释下题目：
> 找出所给数组中能够使解最大的子串，要求时间复杂度为O(n)


## 1. 动态规划求解
#### 实际耗时：13ms
```
public int maxSubArray(int[] nums) {
        int length = nums.length;
        //创建一个新数组，该数组存放到目前为止最大的
        int[] maxArray = new int[length];
        maxArray[0] = nums[0];
        int max = maxArray[0];

        for (int i = 1; i < length; i++) {
            maxArray[i] = nums[i] + (maxArray[i - 1] > 0 ? maxArray[i - 1] : 0);
            max = Math.max(max, maxArray[i]);
        }

        return max;
    }
```
&emsp;&emsp;新建一个叫`maxArray`的数组，用途是来存放最大的和，或者存放nums的数，取决于实际用途。如果我想求整个数组的最大子串的和，那我需要知道`nums[]`排除最后一个数字的最大子串的和，然后单独判断最后一个数字，如果最后一个数字是负数，那么排除掉最后一个数字的就是最大数，否则就加上最后一个数是最大子串。这样以此类推就可以得到整个数组只剩下一个数的情况。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
## 2. 朴实无华法
#### 实际耗时：9ms
```
public int maxSubArray2(int[] nums) {
        int max = Integer.MIN_VALUE;
        int sum = 0;
        for (int i = 0; i < nums.length; i++) {
            if (sum < 0) {
                sum = nums[i];
            } else {
                sum += nums[i];
            }
            if (sum > max) {
                max = sum;
            }
        }
        return max;
    }
```
&emsp;&emsp;代码极其简单，思路也很简单。想求最大的子串的和？很简答，如果加上负数，肯定是亏的，那么我们只需要在`sum<0`的情况下给它赋值`nums[i]`，相当于前面的那堆数字被我全部删掉了，因为加上它们只能是累赘。(**注意**，因为是顺序判断的，所以其实前面的所有的和已经求过了，不会出现这种情况：` -5 7 8 -100` 这四个数字其实最大是15，而加起来是负的，不会对之后的有干扰。)，这样不停下去即可求解。显然这个方法比动态规划好，不需要额外的空间，算法简单。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
