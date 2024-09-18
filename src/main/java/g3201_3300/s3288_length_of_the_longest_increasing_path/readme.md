[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3288\. Length of the Longest Increasing Path

Hard

You are given a 2D array of integers `coordinates` of length `n` and an integer `k`, where `0 <= k < n`.

<code>coordinates[i] = [x<sub>i</sub>, y<sub>i</sub>]</code> indicates the point <code>(x<sub>i</sub>, y<sub>i</sub>)</code> in a 2D plane.

An **increasing path** of length `m` is defined as a list of points <code>(x<sub>1</sub>, y<sub>1</sub>)</code>, <code>(x<sub>2</sub>, y<sub>2</sub>)</code>, <code>(x<sub>3</sub>, y<sub>3</sub>)</code>, ..., <code>(x<sub>m</sub>, y<sub>m</sub>)</code> such that:

*   <code>x<sub>i</sub> < x<sub>i + 1</sub></code> and <code>y<sub>i</sub> < y<sub>i + 1</sub></code> for all `i` where `1 <= i < m`.
*   <code>(x<sub>i</sub>, y<sub>i</sub>)</code> is in the given coordinates for all `i` where `1 <= i <= m`.

Return the **maximum** length of an **increasing path** that contains `coordinates[k]`.

**Example 1:**

**Input:** coordinates = \[\[3,1],[2,2],[4,1],[0,0],[5,3]], k = 1

**Output:** 3

**Explanation:**

`(0, 0)`, `(2, 2)`, `(5, 3)` is the longest increasing path that contains `(2, 2)`.

**Example 2:**

**Input:** coordinates = \[\[2,1],[7,0],[5,6]], k = 2

**Output:** 2

**Explanation:**

`(2, 1)`, `(5, 6)` is the longest increasing path that contains `(5, 6)`.

**Constraints:**

*   <code>1 <= n == coordinates.length <= 10<sup>5</sup></code>
*   `coordinates[i].length == 2`
*   <code>0 <= coordinates[i][0], coordinates[i][1] <= 10<sup>9</sup></code>
*   All elements in `coordinates` are **distinct**.
*   `0 <= k <= n - 1`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int maxPathLength(int[][] coordinates, int k) {
        List<int[]> upper = new ArrayList<>();
        List<int[]> lower = new ArrayList<>();
        for (int[] pair : coordinates) {
            if (pair[0] > coordinates[k][0] && pair[1] > coordinates[k][1]) {
                upper.add(pair);
            }
            if (pair[0] < coordinates[k][0] && pair[1] < coordinates[k][1]) {
                lower.add(pair);
            }
        }
        upper.sort(
                (a, b) -> {
                    if (a[0] == b[0]) {
                        return b[1] - a[1];
                    } else {
                        return a[0] - b[0];
                    }
                });
        lower.sort(
                (a, b) -> {
                    if (a[0] == b[0]) {
                        return b[1] - a[1];
                    } else {
                        return a[0] - b[0];
                    }
                });
        return longestIncreasingLength(upper) + longestIncreasingLength(lower) + 1;
    }

    private int longestIncreasingLength(List<int[]> array) {
        List<Integer> list = new ArrayList<>();
        for (int[] pair : array) {
            int m = list.size();
            if (m == 0 || list.get(m - 1) < pair[1]) {
                list.add(pair[1]);
            } else {
                int idx = binarySearch(list, pair[1]);
                list.set(idx, pair[1]);
            }
        }
        return list.size();
    }

    private int binarySearch(List<Integer> list, int target) {
        int n = list.size();
        int left = 0;
        int right = n - 1;
        while (left < right) {
            int mid = (left + right) / 2;
            if (list.get(mid) == target) {
                return mid;
            } else if (list.get(mid) > target) {
                right = mid;
            } else {
                left = mid + 1;
            }
        }
        return left;
    }
}
```