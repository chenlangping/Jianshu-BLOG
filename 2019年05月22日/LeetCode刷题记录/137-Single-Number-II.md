> Given a **non-empty** array of integers, every element appears three times except for one, which appears exactly once. Find that single one.
#### Example：
> Input: [2,2,3,2]
Output: 3

> Input: [0,1,0,1,0,1,99]
Output: 99
#### Note：
> Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

#### 解释下题目：
> 数组中有一个数字只出现了一次，另外的数字都出现了三次，找出这个仅仅出现一次的数字。


## 1. 用字典做
#### 实际耗时：5ms
```java
public int singleNumber(int[] nums) {
        HashMap<Integer, Integer> hashMap = new HashMap<>(10);
        for (int num : nums) {
            if (hashMap.containsKey(num)) {
                hashMap.put(num, hashMap.get(num) + 1);
            } else {
                hashMap.put(num, 1);
            }
        }

        for (Map.Entry<Integer, Integer> entry : hashMap.entrySet()) {
            if (entry.getValue() == 1) {
                return entry.getKey();
            }
        }
        return -1;
    }
```
&emsp;&emsp;思路很简单啊，用数字作为键，然后出现一次就把value+1，最后找到那个仅出现一次的数字即可。但是很明显，题目中说不要额外的空间，这种解法是不满足的
###### 时间复杂度O(n)
###### 空间复杂度O(n)
---------
## 1. 位操作
#### 实际耗时：0ms
```java
public int singleNumber(int[] nums) {
        int i1 = 0, i2 = 0;
        for (int num : nums) {
            i1 = (i1 ^ num) & ~i2;
            i2 = (i2 ^ num) & ~i1;
        }
        return i1;
    }
```
&emsp;&emsp;思路请见：[https://blog.csdn.net/wlwh90/article/details/89712795](https://blog.csdn.net/wlwh90/article/details/89712795)

###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
