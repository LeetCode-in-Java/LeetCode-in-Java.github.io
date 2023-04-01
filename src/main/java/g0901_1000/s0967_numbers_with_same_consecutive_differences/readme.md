[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 967\. Numbers With Same Consecutive Differences

Medium

Return all **non-negative** integers of length `n` such that the absolute difference between every two consecutive digits is `k`.

Note that **every** number in the answer **must not** have leading zeros. For example, `01` has one leading zero and is invalid.

You may return the answer in **any order**.

**Example 1:**

**Input:** n = 3, k = 7

**Output:** [181,292,707,818,929]

**Explanation:** Note that 070 is not a valid number, because it has leading zeroes.

**Example 2:**

**Input:** n = 2, k = 1

**Output:** [10,12,21,23,32,34,43,45,54,56,65,67,76,78,87,89,98]

**Constraints:**

*   `2 <= n <= 9`
*   `0 <= k <= 9`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int[] numsSameConsecDiff(int n, int k) {
        k = Math.abs(k);
        List<Integer> list = new ArrayList<>();
        dfs(list, 100000, 0, n, k);
        int[] res = new int[list.size()];
        for (int i = 0; i < res.length; i++) {
            res[i] = list.get(i);
        }
        return res;
    }

    private void dfs(List<Integer> list, int can, int len, int n, int k) {
        if (len == n) {
            list.add(can);
            return;
        }
        if (can == 0) {
            return;
        }
        if (len == 0) {
            for (int i = 0; i <= 9; i++) {
                dfs(list, i, 1, n, k);
            }
        } else {
            int last = can % 10;
            int a = last + k;
            int b = last - k;
            if (b >= 0) {
                dfs(list, can * 10 + b, len + 1, n, k);
            }
            if (k != 0 && a < 10) {
                dfs(list, can * 10 + a, len + 1, n, k);
            }
        }
    }
}
```