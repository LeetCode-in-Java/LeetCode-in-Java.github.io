[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 757\. Set Intersection Size At Least Two

Hard

An integer interval `[a, b]` (for integers `a < b`) is a set of all consecutive integers from `a` to `b`, including `a` and `b`.

Find the minimum size of a set S such that for every integer interval A in `intervals`, the intersection of S with A has a size of at least two.

**Example 1:**

**Input:** intervals = \[\[1,3],[1,4],[2,5],[3,5]]

**Output:** 3

**Explanation:**

    Consider the set S = {2, 3, 4}. For each interval, there are at least 2 elements from S in the interval.
    Also, there isn't a smaller size set that fulfills the above condition.
    Thus, we output the size of this set, which is 3. 

**Example 2:**

**Input:** intervals = \[\[1,2],[2,3],[2,4],[4,5]]

**Output:** 5

**Explanation:** An example of a minimum sized set is {1, 2, 3, 4, 5}.

**Constraints:**

*   `1 <= intervals.length <= 3000`
*   `intervals[i].length == 2`
*   <code>0 <= a<sub>i</sub> < b<sub>i</sub> <= 10<sup>8</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int intersectionSizeTwo(int[][] intervals) {
        int n = 0;
        long[] endStartPairs = new long[intervals.length];
        for (int[] interval : intervals) {
            endStartPairs[n] = -interval[0] & 0xFFFFFFFFL;
            endStartPairs[n++] |= (long) (interval[1]) << 32;
        }
        Arrays.sort(endStartPairs);
        int min = -2;
        int max = -1;
        int curStart;
        int curEnd;
        int res = 0;
        for (long endStartPair : endStartPairs) {
            curStart = -(int) endStartPair;
            curEnd = (int) (endStartPair >> 32);
            if (curStart <= min) {
                continue;
            }
            if (curStart <= max) {
                res += 1;
                min = max;
            } else {
                res += 2;
                min = curEnd - 1;
            }
            max = curEnd;
        }
        return res;
    }
}
```