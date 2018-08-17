>Given an array of integers, return indices of the two numbers such that they add up to a specific target.<br>
You may assume that each input would have exactly one solution, and you may not use the same element twice.
#### Example:
> Given nums = [2, 7, 11, 15], target = 9,
Because nums[0] + nums[1] = 2 + 7 = 9,
return [0, 1].

就是给定一个数组nums[]和一个目标数字target，找出数组中的两个数相加能是target的数，返回下标的数组

## 1. 简单的暴力求解 
#### 实际耗时：23ms
```
public int[] twoSum(int[] nums, int target) {
        for (int i = 0 ;i<nums.length;i++){
            for (int j = i+1 ;j<nums.length ; j ++){
                if (target == (nums[i]+nums[j])){
                    return new int[] {i,j};
                }
            }
        }
        return null;
    }
```
就遍历两遍就行了。
###### 时间复杂度O(n^2)  空间复杂度O(1)
---------
## 2.利用java自带的HashMap
```
public int[] twoSum2(int[] nums, int target) {
        //1.首先建立hashmap
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        //2.建立映射关系
        //需要注意的是，应该把nums[i]作为key，下标作为value，因为需要根据key来找value
        for (int i = 0; i < nums.length; i++) {
            map.put(nums[i], i);
        }
        //3.利用hashmap快速查找
        for (int i = 0; i < nums.length; i++) {
            int temp = target - nums[i];
            //map中包含那个数字且不是那个数字自己
            if (map.containsKey(temp) && map.get(temp) != i) {
                return new int[]{i, map.get(temp)};
            }
        }
        return null;
    }
```
思路都在注释之中，需要注意的是照理来说应该是`map.put(i,nums[i])`，因为Map的key是不可以重复的，为什么要把可能重复的`nums[i]`作为key呢？因为题中说了有且仅有一个解。所以假设`nums[i]`和`nums[j]`都是解且它们俩相等，那么就说明有两个解。而且`map.put(nums[i],i)`的好处是我可以通过`nums[i]`轻松找到`i`
##### 时间复杂度O(n)，空间复杂度O(n)，标准的牺牲空间换取时间。
----------
## 3.HashMap边处理边判断
其实就是方法2中把先放入map中然后 进行判断的思路整合到一起，一边判断一边放入Map中。
#### 实际耗时：8ms
```
public int[] twoSum3(int[] nums, int target) {
        //1.首先建立hashmap
        Map<Integer, Integer> map = new HashMap<Integer, Integer>();
        //2.利用hashmap快速查找
        for (int i = 0; i < nums.length; i++) {
            int temp = target - nums[i];
            if (map.containsKey(temp)) {
                return new int[]{map.get(temp),i };
            }
            map.put(nums[i],i);
        }
        return null;
    }
```
###### 时间复杂度O(n)  空间复杂度O(n)，比2稍微快那么一点点，因为可以直接找到就返回，而不用把所有的元素都放进去。
