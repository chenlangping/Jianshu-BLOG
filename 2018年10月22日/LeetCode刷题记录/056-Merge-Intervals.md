> Given a collection of intervals, merge all overlapping intervals.

tips:这题是北航2017年推免生的机试题
#### Example：
> Input: [[1,3],[2,6],[8,10],[15,18]]
Output: [[1,6],[8,10],[15,18]]
Explanation: Since intervals [1,3] and [2,6] overlaps, merge them into [1,6].

> Input: [[1,4],[4,5]]
Output: [[1,5]]
Explanation: Intervals [1,4] and [4,5] are considerred overlapping.

#### 解释下题目：
> 给定一些区间，然后求出这些区间的最大区间。


## 1. 先按照起始位置排序，然后求解
#### 实际耗时：15ms
```
public List<Interval> merge(List<Interval> intervals) {
        //因为之后有intervals.get(0).start，所以需要排除Null
        if (intervals.isEmpty()) {
            return intervals;
        }
        //先按start从小到大排
        Collections.sort(intervals, new IntervalComparator());
        List<Interval> result = new ArrayList<>();
        //result_start记录最大的区间的起始位置，result_end记录最大的区间末尾位置
        int result_start = intervals.get(0).start;
        int result_end = intervals.get(0).end;
        for (int i = 1; i < intervals.size(); i++) {
            if (intervals.get(i - 1).start == intervals.get(i).start) {
                //找到相同的start中end最大的，就是整个区间最长的
                result_end = Math.max(intervals.get(i).end, result_end);
                continue;
            } else {
                //此时两个区间完全没有交集
                if (intervals.get(i).start > result_end) {
                    //把前一个放进去
                    result.add(new Interval(result_start, result_end));
                    //把这两个值赋予初值，开始下一轮循环
                    result_start = intervals.get(i).start;
                    result_end = intervals.get(i).end;
                } else {
                    //说明这两个的区间是有交集的
                    result_end = Math.max(intervals.get(i).end, result_end);
                }
            }
        }
        //记得把最后的一个区间放进去
        result.add(new Interval(result_start, result_end));
        return result;
    }

    //搞个比较器类，start小的排前面
    private class IntervalComparator implements Comparator<Interval> {
        @Override
        public int compare(Interval o1, Interval o2) {
            if (o1.start < o2.start) {
                return -1;
            } else if (o1.start > o2.start) {
                return 1;
            } else return 0;


            //return a.start < b.start ? -1 : a.start == b.start ? 0 : 1;
        }
    }
```
###### 踩过的坑：什么都没有的list，还有比较器不可太复杂，否则好像不让过
&emsp;&emsp;思路，首先按照start从小到大开始排序，这样之后如果之后如果有相同的start的，那么它们肯定是相邻的，只需要找出这些相同start的区间的最大的end即可。如果发现有个区间的start和之前的不同，那么只需要判断这个区间的start是否落入之前的区间，如果落入那么就扩展最大区间，如果没有落入说明之前的那个就是最大区间。最后不要忘了把最后一个区间加进去。
###### 时间复杂度O(nlogn)
###### 空间复杂度O(1)
---------

