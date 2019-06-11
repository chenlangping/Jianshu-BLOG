> There are N gas stations along a circular route, where the amount of gas at station i is gas[i].
You have a car with an unlimited gas tank and it costs cost[i] of gas to travel from station i to its next station (i+1). You begin the journey with an empty tank at one of the gas stations.
Return the starting gas station's index if you can travel around the circuit once in the clockwise direction, otherwise return -1.
#### Example：
> Input: 
gas  = [1,2,3,4,5]
cost = [3,4,5,1,2]
Output: 3
Explanation:
Start at station 3 (index 3) and fill up with 4 unit of gas. Your tank = 0 + 4 = 4
Travel to station 4. Your tank = 4 - 1 + 5 = 8
Travel to station 0. Your tank = 8 - 2 + 1 = 7
Travel to station 1. Your tank = 7 - 3 + 2 = 6
Travel to station 2. Your tank = 6 - 4 + 3 = 5
Travel to station 3. The cost is 5. Your gas is just enough to travel back to station 3.
Therefore, return 3 as the starting index.
#### Note：
> If there exists a solution, it is guaranteed to be unique.
Both input arrays are non-empty and have the same length.
Each element in the input arrays is a non-negative integer.

#### 解释下题目：
> 加油站题目，就是你到达这个加油站可以加油（你的邮箱是无限大的），然后需要花费对应的油量，计算从哪个站点出发能环游一周。


## 1. 计算“利润”
#### 实际耗时：1ms
```java
public int canCompleteCircuit(int[] gas, int[] cost) {
        if (gas == null || cost == null || gas.length == 0 || cost.length == 0) {
            return -1;
        }
        int[] res = new int[gas.length];
        int gasSum = 0;
        int costSum = 0;
        for (int i = 0; i < gas.length; i++) {
            res[i] = gas[i] - cost[i];
            gasSum += gas[i];
            costSum += cost[i];
        }

        if (gasSum < costSum) {
            return -1;
        }

        int result = 0;
        int curSum = 0;
        for (int i = 0; i < gas.length; i++) {
            if (res[i] == 0) {
                continue;
            }
            if (curSum == 0 && res[i] > 0) {
                curSum = res[i];
                result = i;
            } else if (curSum == 0 && res[i] < 0) {
                result = -1;
            } else if (curSum < 0 && res[i] > 0) {
                curSum = res[i];
                result = i;
            } else if (curSum < 0 && res[i] < 0) {
                curSum = 0;
                result = -1;
            } else if (curSum > 0 && res[i] > 0) {
                curSum += res[i];
            } else {
                if (curSum + res[i] >= 0) {
                    curSum += res[i];
                } else {
                    curSum = 0;
                    result = -1;
                }
            }
        }

        return result;

    }
```
&emsp;&emsp;思路就是把你赚到的减掉消耗的，如果是大于0的话说明到这个站你的油会增加，否则油量会少下去（不划算）。但是说实话我写的不太好，一堆复杂的判断
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
## 2. 思路和方法1一样，只不过优化了代码
#### 实际耗时：xxms
```java
public int canCompleteCircuit2(int[] gas, int[] cost) {
        int tank = 0;
        for (int i = 0; i < gas.length; i++) {
            tank += gas[i] - cost[i];
        }
        if (tank < 0) {
            return -1;
        }

        int start = 0;
        int accumulate = 0;
        for (int i = 0; i < gas.length; i++) {
            int curGain = gas[i] - cost[i];
            if (accumulate + curGain < 0) {
                start = i + 1;
                accumulate = 0;
            } else {
                accumulate += curGain;
            }
        }

        return start;
    }
```
优化后的方法1
###### 时间复杂度O(n)
###### 空间复杂度O(1)
---------
