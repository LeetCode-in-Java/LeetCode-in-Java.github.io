[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3414\. Maximum Score of Non-overlapping Intervals

Hard

You are given a 2D integer array `intervals`, where <code>intervals[i] = [l<sub>i</sub>, r<sub>i</sub>, weight<sub>i</sub>]</code>. Interval `i` starts at position <code>l<sub>i</sub></code> and ends at <code>r<sub>i</sub></code>, and has a weight of <code>weight<sub>i</sub></code>. You can choose _up to_ 4 **non-overlapping** intervals. The **score** of the chosen intervals is defined as the total sum of their weights.

Return the **lexicographically smallest** array of at most 4 indices from `intervals` with **maximum** score, representing your choice of non-overlapping intervals.

Two intervals are said to be **non-overlapping** if they do not share any points. In particular, intervals sharing a left or right boundary are considered overlapping.

An array `a` is **lexicographically smaller** than an array `b` if in the first position where `a` and `b` differ, array `a` has an element that is less than the corresponding element in `b`.   
 If the first `min(a.length, b.length)` elements do not differ, then the shorter array is the lexicographically smaller one.

**Example 1:**

**Input:** intervals = \[\[1,3,2],[4,5,2],[1,5,5],[6,9,3],[6,7,1],[8,9,1]]

**Output:** [2,3]

**Explanation:**

You can choose the intervals with indices 2, and 3 with respective weights of 5, and 3.

**Example 2:**

**Input:** intervals = \[\[5,8,1],[6,7,7],[4,7,3],[9,10,6],[7,8,2],[11,14,3],[3,5,5]]

**Output:** [1,3,5,6]

**Explanation:**

You can choose the intervals with indices 1, 3, 5, and 6 with respective weights of 7, 6, 3, and 5.

**Constraints:**

*   <code>1 <= intevals.length <= 5 * 10<sup>4</sup></code>
*   `intervals[i].length == 3`
*   <code>intervals[i] = [l<sub>i</sub>, r<sub>i</sub>, weight<sub>i</sub>]</code>
*   <code>1 <= l<sub>i</sub> <= r<sub>i</sub> <= 10<sup>9</sup></code>
*   <code>1 <= weight<sub>i</sub> <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.List;

public class Solution {
    public int[] maximumWeight(List<List<Integer>> intervals) {
        final int n = intervals.size();
        final int[][] ns = new int[n][];
        int p = 0;
        for (List<Integer> li : intervals) {
            ns[p] = new int[] {li.get(0), li.get(1), li.get(2), p};
            p++;
        }
        int[][] dp1 = new int[n][0];
        long[] dp = new long[n];
        Arrays.sort(ns, (a, b) -> (a[0] - b[0]));
        for (int k = 0; k < 4; ++k) {
            int[][] dp3 = new int[n][];
            long[] dp2 = new long[n];
            dp3[n - 1] = new int[] {ns[n - 1][3]};
            dp2[n - 1] = ns[n - 1][2];
            for (int i = n - 2; i >= 0; --i) {
                int l = i + 1;
                int r = n - 1;
                while (l <= r) {
                    final int mid = (l + r) >> 1;
                    if (ns[mid][0] > ns[i][1]) {
                        r = mid - 1;
                    } else {
                        l = mid + 1;
                    }
                }
                dp2[i] = ns[i][2] + (l < n ? dp[l] : 0);
                if (i + 1 < n && dp2[i + 1] > dp2[i]) {
                    dp2[i] = dp2[i + 1];
                    dp3[i] = dp3[i + 1];
                } else {
                    if (l < n) {
                        dp3[i] = new int[dp1[l].length + 1];
                        dp3[i][0] = ns[i][3];
                        for (int j = 0; j < dp1[l].length; ++j) {
                            dp3[i][j + 1] = dp1[l][j];
                        }
                        Arrays.sort(dp3[i]);
                    } else {
                        dp3[i] = new int[] {ns[i][3]};
                    }
                    if (i + 1 < n && dp2[i + 1] == dp2[i] && check(dp3[i], dp3[i + 1]) > 0) {
                        dp3[i] = dp3[i + 1];
                    }
                }
            }
            dp = dp2;
            dp1 = dp3;
        }
        return dp1[0];
    }

    private int check(final int[] ns1, final int[] ns2) {
        int i = 0;
        while (i < ns1.length && i < ns2.length) {
            if (ns1[i] < ns2[i]) {
                return -1;
            } else if (ns1[i] > ns2[i]) {
                return 1;
            }
            i++;
        }
        return 0;
    }
}
```