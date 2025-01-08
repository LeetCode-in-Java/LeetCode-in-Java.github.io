[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3394\. Check if Grid can be Cut into Sections

Medium

You are given an integer `n` representing the dimensions of an `n x n` grid, with the origin at the bottom-left corner of the grid. You are also given a 2D array of coordinates `rectangles`, where `rectangles[i]` is in the form <code>[start<sub>x</sub>, start<sub>y</sub>, end<sub>x</sub>, end<sub>y</sub>]</code>, representing a rectangle on the grid. Each rectangle is defined as follows:

*   <code>(start<sub>x</sub>, start<sub>y</sub>)</code>: The bottom-left corner of the rectangle.
*   <code>(end<sub>x</sub>, end<sub>y</sub>)</code>: The top-right corner of the rectangle.

**Note** that the rectangles do not overlap. Your task is to determine if it is possible to make **either two horizontal or two vertical cuts** on the grid such that:

*   Each of the three resulting sections formed by the cuts contains **at least** one rectangle.
*   Every rectangle belongs to **exactly** one section.

Return `true` if such cuts can be made; otherwise, return `false`.

**Example 1:**

**Input:** n = 5, rectangles = \[\[1,0,5,2],[0,2,2,4],[3,2,5,3],[0,4,4,5]]

**Output:** true

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/10/23/tt1drawio.png)

The grid is shown in the diagram. We can make horizontal cuts at `y = 2` and `y = 4`. Hence, output is true.

**Example 2:**

**Input:** n = 4, rectangles = \[\[0,0,1,1],[2,0,3,4],[0,2,2,3],[3,0,4,3]]

**Output:** true

**Explanation:**

![](https://assets.leetcode.com/uploads/2024/10/23/tc2drawio.png)

We can make vertical cuts at `x = 2` and `x = 3`. Hence, output is true.

**Example 3:**

**Input:** n = 4, rectangles = \[\[0,2,2,4],[1,0,3,2],[2,2,3,4],[3,0,4,2],[3,2,4,4]]

**Output:** false

**Explanation:**

We cannot make two horizontal or two vertical cuts that satisfy the conditions. Hence, output is false.

**Constraints:**

*   <code>3 <= n <= 10<sup>9</sup></code>
*   <code>3 <= rectangles.length <= 10<sup>5</sup></code>
*   `0 <= rectangles[i][0] < rectangles[i][2] <= n`
*   `0 <= rectangles[i][1] < rectangles[i][3] <= n`
*   No two rectangles overlap.

## Solution

```java
import java.util.Arrays;

@SuppressWarnings({"unused", "java:S1172"})
public class Solution {
    private static final int MASK = (1 << 30) - 1;

    public boolean checkValidCuts(int m, int[][] rectangles) {
        int n = rectangles.length;
        long[] start = new long[n];
        for (int i = 0; i < n; i++) {
            start[i] = ((long) rectangles[i][1] << 32) + rectangles[i][3];
        }
        Arrays.sort(start);
        if (validate(start)) {
            return true;
        }
        for (int i = 0; i < n; i++) {
            start[i] = ((long) rectangles[i][0] << 32) + rectangles[i][2];
        }
        Arrays.sort(start);
        return validate(start);
    }

    private boolean validate(long[] arr) {
        int cut = 0;
        int n = arr.length;
        int max = (int) arr[0] & MASK;
        for (int i = 0; i < n; i++) {
            int start = (int) (arr[i] >> 32);
            if (start >= max && ++cut == 2) {
                return true;
            }
            max = Math.max(max, (int) (arr[i] & MASK));
        }
        return false;
    }
}
```