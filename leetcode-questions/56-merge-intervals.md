# 56 Merge Intervals

## Leetcode 56 Merge Intervals

Written: 2020-12-21

\#heap \#priority queue \#dynamic programming

 Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

### Method: Dynamic Programming

{% hint style="info" %}
Sort the intervals by their starting value, and then merge if the following interval is overlapping.
{% endhint %}

```java
class Solution {
    public int[][] merge(int[][] intervals) {
        // Special case
        if (intervals == null || intervals.length == 0) {
            return intervals;
        }
        
        // Step 1: Sort the intervals based on start
        Collections.sort(Arrays.asList(intervals), (a, b) -> {
            return a[0] - b[0];
        });
        
        List<int[]> result = new ArrayList<>();
        result.add(intervals[0]);
        int currEnd = intervals[0][1];
        
        // Step 2: Merge if applicable
        int index = 1;
        while (index < intervals.length) {
            int[] curr = intervals[index];
            if (curr[0] <= currEnd) {
                // can be merged, update current end
                currEnd = Math.max(curr[1], currEnd);
            } else {
                // cannot be merged, start a new one
                result.get(result.size() - 1)[1] = currEnd;
                result.add(curr);
                currEnd = curr[1];
            }
            index++;
        }
        result.get(result.size() - 1)[1] = currEnd;
        
        // Step 3: Put inside an 2D array
        int[][] res = new int[result.size()][];
        for (int i = 0; i < result.size(); i++) {
            res[i] = result.get(i);
        }
        
        return res;
    }
}
```

