[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1893\. Check if All the Integers in a Range Are Covered

Easy

You are given a 2D integer array `ranges` and two integers `left` and `right`. Each <code>ranges[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> represents an **inclusive** interval between <code>start<sub>i</sub></code> and <code>end<sub>i</sub></code>.

Return `true` _if each integer in the inclusive range_ `[left, right]` _is covered by **at least one** interval in_ `ranges`. Return `false` _otherwise_.

An integer `x` is covered by an interval <code>ranges[i] = [start<sub>i</sub>, end<sub>i</sub>]</code> if <code>start<sub>i</sub> <= x <= end<sub>i</sub></code>.

**Example 1:**

**Input:** ranges = \[\[1,2],[3,4],[5,6]], left = 2, right = 5

**Output:** true

**Explanation:** Every integer between 2 and 5 is covered:

- 2 is covered by the first range.

- 3 and 4 are covered by the second range.

- 5 is covered by the third range. 

**Example 2:**

**Input:** ranges = \[\[1,10],[10,20]], left = 21, right = 21

**Output:** false

**Explanation:** 21 is not covered by any range. 

**Constraints:**

*   `1 <= ranges.length <= 50`
*   <code>1 <= start<sub>i</sub> <= end<sub>i</sub> <= 50</code>
*   `1 <= left <= right <= 50`

## Solution

```java
public class Solution {
    public boolean isCovered(int[][] ranges, int left, int right) {
        int[] temp = new int[52];
        for (int[] range : ranges) {
            int start = range[0];
            int end = range[ranges[0].length - 1];
            temp[start] += 1;
            temp[end + 1] -= 1;
        }
        for (int i = 1; i < temp.length; i++) {
            temp[i] += temp[i - 1];
        }
        for (int i = left; i <= right; i++) {
            if (temp[i] == 0) {
                return false;
            }
        }
        return true;
    }
}
```