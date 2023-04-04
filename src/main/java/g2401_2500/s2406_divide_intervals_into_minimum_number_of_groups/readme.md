[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2406\. Divide Intervals Into Minimum Number of Groups

Medium

You are given a 2D integer array `intervals` where <code>intervals[i] = [left<sub>i</sub>, right<sub>i</sub>]</code> represents the **inclusive** interval <code>[left<sub>i</sub>, right<sub>i</sub>]</code>.

You have to divide the intervals into one or more **groups** such that each interval is in **exactly** one group, and no two intervals that are in the same group **intersect** each other.

Return _the **minimum** number of groups you need to make_.

Two intervals **intersect** if there is at least one common number between them. For example, the intervals `[1, 5]` and `[5, 8]` intersect.

**Example 1:**

**Input:** intervals = \[\[5,10],[6,8],[1,5],[2,3],[1,10]]

**Output:** 3

**Explanation:** We can divide the intervals into the following groups:

- Group 1: [1, 5], [6, 8].

- Group 2: [2, 3], [5, 10].

- Group 3: [1, 10].

It can be proven that it is not possible to divide the intervals into fewer than 3 groups. 

**Example 2:**

**Input:** intervals = \[\[1,3],[5,6],[8,10],[11,13]]

**Output:** 1

**Explanation:** None of the intervals overlap, so we can put all of them in one group. 

**Constraints:**

*   <code>1 <= intervals.length <= 10<sup>5</sup></code>
*   `intervals[i].length == 2`
*   <code>1 <= left<sub>i</sub> <= right<sub>i</sub> <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public int minGroups(int[][] intervals) {
        int maxElement = 0;
        for (int[] i : intervals) {
            maxElement = Math.max(maxElement, i[0]);
            maxElement = Math.max(maxElement, i[1]);
        }
        long[] prefixSum = new long[maxElement + 2];
        for (int[] i : intervals) {
            prefixSum[i[0]] += 1;
            prefixSum[i[1] + 1] -= 1;
        }
        long ans = 0;
        for (int i = 1; i <= maxElement + 1; i++) {
            prefixSum[i] += prefixSum[i - 1];
            ans = Math.max(ans, prefixSum[i]);
        }
        return (int) ans;
    }
}
```