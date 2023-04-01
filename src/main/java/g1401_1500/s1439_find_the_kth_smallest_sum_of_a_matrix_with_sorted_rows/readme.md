[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1439\. Find the Kth Smallest Sum of a Matrix With Sorted Rows

Hard

You are given an `m x n` matrix `mat` that has its rows sorted in non-decreasing order and an integer `k`.

You are allowed to choose **exactly one element** from each row to form an array.

Return _the_ <code>k<sup>th</sup></code> _smallest array sum among all possible arrays_.

**Example 1:**

**Input:** mat = \[\[1,3,11],[2,4,6]], k = 5

**Output:** 7

**Explanation:** Choosing one element from each row, the first k smallest sum are: [1,2], [1,4], [3,2], [3,4], [1,6]. Where the 5th sum is 7.

**Example 2:**

**Input:** mat = \[\[1,3,11],[2,4,6]], k = 9

**Output:** 17

**Example 3:**

**Input:** mat = \[\[1,10,10],[1,4,5],[2,3,6]], k = 7

**Output:** 9

**Explanation:** Choosing one element from each row, the first k smallest sum are: [1,1,2], [1,1,3], [1,4,2], [1,4,3], [1,1,6], [1,5,2], [1,5,3]. Where the 7th sum is 9.

**Constraints:**

*   `m == mat.length`
*   `n == mat.length[i]`
*   `1 <= m, n <= 40`
*   `1 <= mat[i][j] <= 5000`
*   <code>1 <= k <= min(200, n<sup>m</sup>)</code>
*   `mat[i]` is a non-decreasing array.

## Solution

```java
import java.util.Arrays;
import java.util.Objects;
import java.util.TreeSet;

public class Solution {
    public int kthSmallest(int[][] mat, int k) {
        TreeSet<int[]> treeSet =
                new TreeSet<>(
                        (o1, o2) -> {
                            if (o1[0] != o2[0]) {
                                return o1[0] - o2[0];
                            } else {
                                for (int i = 1; i < o1.length; i++) {
                                    if (o1[i] != o2[i]) {
                                        return o1[i] - o2[i];
                                    }
                                }
                                return 0;
                            }
                        });
        int m = mat.length;
        int n = mat[0].length;
        int sum = 0;
        int[] entry = new int[m + 1];
        for (int[] ints : mat) {
            sum += ints[0];
        }
        entry[0] = sum;
        treeSet.add(entry);
        int count = 0;
        while (count < k) {
            int[] curr = treeSet.pollFirst();
            count++;
            if (count == k) {
                return Objects.requireNonNull(curr)[0];
            }
            for (int i = 0; i < m; i++) {
                int[] next = Arrays.copyOf(Objects.requireNonNull(curr), curr.length);
                if (curr[i + 1] + 1 < n) {
                    next[0] -= mat[i][curr[i + 1]];
                    next[0] += mat[i][curr[i + 1] + 1];
                    next[i + 1]++;
                    treeSet.add(next);
                }
            }
        }
        return -1;
    }
}
```