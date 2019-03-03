> The set [1,2,3,...,n] contains a total of n! unique permutations.
By listing and labeling all of the permutations in order, we get the following sequence for n = 3:
"123"
"132"
"213"
"231"
"312"
"321"
Given n and k, return the kth permutation sequence.
#### Example：
> Input: n = 3, k = 3
Output: "213"

> Input: n = 4, k = 9
Output: "2314"
#### Note：
> Given n will be between 1 and 9 inclusive.
Given k will be between 1 and n! inclusive.

#### 解释下题目：
> 给定一个数字n(n<=9)，找出n的全排列的第k个数是多少


## 1. 笨办法，把全排列搞出来
#### 实际耗时：超时
```
public String getPermutation(int n, int k) {
        int nums[] = new int[n];
        int des[] = new int[1];
        int cur[] = new int[1];
        cur[0] = 0;
        des[0] = k;
        for (int i = 0; i < n; i++) {
            nums[i] = i + 1;
        }
        String s[] = new String[1];
        recursion(nums, cur, des, s, new ArrayList<>());
        return s[0];
    }

    private void recursion(int nums[], int[] cur, int[] des, String s[], List<Integer> tempList) {
        if (tempList.size() == nums.length) {
            if (cur[0] == des[0] - 1) {
                StringBuilder sb = new StringBuilder();
                for (int j = 0; j < tempList.size(); j++) {
                    sb.append(tempList.get(j));
                }
                s[0] = sb.toString();
                cur[0]++;
                return;
            } else {
                cur[0]++;
            }

        }
        for (int i = 0; i < nums.length; i++) {
            if (tempList.contains(nums[i])) {
                continue;
            }
            tempList.add(nums[i]);
            recursion(nums, cur, des, s, tempList);
            tempList.remove(tempList.size() - 1);
        }
    }
```
&emsp;&emsp;思路很简单，就是找出全排列，然后返回要求的第k个数字就行了。当然这是不可取的！（我连自己的代码都看不懂了......）
###### 时间复杂度O(n!)
###### 空间复杂度O(n!)
---------
## 2. 数学分析
#### 实际耗时：16ms
```
public String getPermutation2(int n, int k) {
        List<Integer> numbers = new ArrayList<>();
        int[] factorial = new int[n + 1];
        StringBuilder sb = new StringBuilder();

        // 创建阶乘数组
        int sum = 1;
        factorial[0] = 1;
        for (int i = 1; i <= n; i++) {
            sum *= i;
            factorial[i] = sum;
        }
        // factorial[] = {1, 1, 2, 6, 24, ... n!}


        for (int i = 1; i <= n; i++) {
            numbers.add(i);
        }

        //因为数组从0开始，所以第1个其实是第0个，所以需要k--
        k--;

        for (int i = 1; i <= n; i++) {
            int index = k / factorial[n - i];
            sb.append(String.valueOf(numbers.get(index)));
            numbers.remove(index);
            k -= index * factorial[n - i];
        }

        return String.valueOf(sb);
    }
```
&emsp;&emsp;思路很简单，假设n=4，那么有4!=24种可能，其中1-6是1开头的，7-12是2开头的，以此类推。这样就可以大幅度缩小查找时间，因为并不需要把所有的都列出来了。
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
