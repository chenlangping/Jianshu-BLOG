> Given a set of *non-overlapping* intervals, insert a new interval into the intervals (merge if necessary).
You may assume that the intervals were initially sorted according to their start times.
#### Example：
> Input: intervals = [[1,3],[6,9]], newInterval = [2,5]
Output: [[1,5],[6,9]]

> Input: intervals = [[1,2],[3,5],[6,7],[8,10],[12,16]], newInterval = [4,8]
Output: [[1,2],[3,10],[12,16]]
Explanation: Because the new interval [4,8] overlaps with [3,5],[6,7],[8,10].


#### 解释下题目：
> 给定一堆区间和一个新区间，融合一下


## 1. 挨个判断
#### 实际耗时：12ms
```
public List<Interval> insert(List<Interval> intervals, Interval newInterval) {
        //开辟新的list用来存放处理之后的结果
        List<Interval> list = new ArrayList<>();
        //如果是空的就直接把新的区间返回去即可
        if (intervals.isEmpty()) {
            list.add(newInterval);
            return list;
        }
        for (int i = 0; i < intervals.size(); i++) {
            if (!judgeOverlap(newInterval, intervals.get(i))) {
                //如果没有交集，直接放进去就行
                list.add(intervals.get(i));
            } else {
                //有交集则扩展这个交集
                newInterval.start = Math.min(newInterval.start, intervals.get(i).start);
                newInterval.end = Math.max(newInterval.end, intervals.get(i).end);
            }
        }
        //最后把融合成的大区间放进去
        list.add(newInterval);
        //排个序
        Collections.sort(list, new IntervalComparator());
        return list;
    }

    /**
     * 判断两个区间是否有重合
     *
     * @param i1 区间1
     * @param i2 区间2
     * @return true=有重合
     */
    public boolean judgeOverlap(Interval i1, Interval i2) {
        if (i1.start <= i2.start && i1.end >= i2.end) {
            return true;
        }
        if (i1.start <= i2.start && i1.end <= i2.end && i1.end >= i2.start) {
            //这里注意有三个判断
            return true;
        }
        if (i1.start >= i2.start && i1.end >= i2.end && i1.start <= i2.end) {
            //这里注意有三个判断
            return true;
        }
        if (i1.start >= i2.start && i1.end <= i2.end) {
            return true;
        }
        return false;
    }

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
###### 踩过的坑：这道题居然要求是最后排好序的一个List 
`[[1,5]]`和`[0,0]`
`[[1,5]]`和`[6,8]`
`[[1,3],[6,9]]`和`[2,5]`

&emsp;&emsp;思路：很简单，每次从List中取出一个区间，判断它和新区间是否有重叠部分，没有就直接把新区间扔到返回的list中，如果有重叠部分，就把这两个区间融合到一起成为新区间。
&emsp;&emsp;这题最后还要再排一个序。当然由题目可知是本来就是已经按照区间起点排好序了的，所以其实可以稍微优化点：
1. 一旦新区间已经和别的区间融合过了，再遇到另外一个区间而且这两个区间并没有交集的时候，那么接下来的就可以不用判断了，因为是肯定不可能再发生融合了的。
2. 可以在放入返回List中的做一些简单判断，这样最后就可以不用排序，省下`nlog(n)`的时间，但是前提是区间必须一开始是有序的（本题说了一开始有序，可以用，但是我懒）。
###### 时间复杂度O(n+nlog(n))
###### 空间复杂度O(1)
---------

