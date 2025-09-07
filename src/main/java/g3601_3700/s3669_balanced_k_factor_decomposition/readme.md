[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3669\. Balanced K-Factor Decomposition

Medium

Given two integers `n` and `k`, split the number `n` into exactly `k` positive integers such that the **product** of these integers is equal to `n`.

Return _any_ _one_ split in which the **maximum** difference between any two numbers is **minimized**. You may return the result in _any order_.

**Example 1:**

**Input:** n = 100, k = 2

**Output:** [10,10]

**Explanation:**

The split `[10, 10]` yields `10 * 10 = 100` and a max-min difference of 0, which is minimal.

**Example 2:**

**Input:** n = 44, k = 3

**Output:** [2,2,11]

**Explanation:**

*   Split `[1, 1, 44]` yields a difference of 43
*   Split `[1, 2, 22]` yields a difference of 21
*   Split `[1, 4, 11]` yields a difference of 10
*   Split `[2, 2, 11]` yields a difference of 9

Therefore, `[2, 2, 11]` is the optimal split with the smallest difference 9.

**Constraints:**

*   <code>4 <= n <= 10<sup>5</sup></code>
*   `2 <= k <= 5`
*   `k` is strictly less than the total number of positive divisors of `n`.

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class Solution {
    private int kGlobal;
    private int bestDiff = Integer.MAX_VALUE;
    private List<Integer> bestList = new ArrayList<>();
    private final List<Integer> current = new ArrayList<>();

    public int[] minDifference(int n, int k) {
        kGlobal = k;
        dfs(n, 1, 0);
        int[] ans = new int[bestList.size()];
        for (int i = 0; i < bestList.size(); i++) {
            ans[i] = bestList.get(i);
        }
        return ans;
    }

    private void dfs(int rem, int start, int depth) {
        if (depth == kGlobal - 1) {
            if (rem >= start) {
                current.add(rem);
                evaluate();
                current.remove(current.size() - 1);
            }
            return;
        }
        List<Integer> divs = getDivisors(rem);
        for (int d : divs) {
            if (d < start) {
                continue;
            }
            current.add(d);
            dfs(rem / d, d, depth + 1);
            current.remove(current.size() - 1);
        }
    }

    private void evaluate() {
        int mn = Integer.MAX_VALUE;
        int mx = Integer.MIN_VALUE;
        for (int v : current) {
            mn = Math.min(mn, v);
            mx = Math.max(mx, v);
        }
        int diff = mx - mn;
        if (diff < bestDiff) {
            bestDiff = diff;
            bestList = new ArrayList<>(current);
        }
    }

    private List<Integer> getDivisors(int x) {
        List<Integer> small = new ArrayList<>();
        List<Integer> large = new ArrayList<>();
        for (int i = 1; i * (long) i <= x; i++) {
            if (x % i == 0) {
                small.add(i);
                if (i != x / i) {
                    large.add(x / i);
                }
            }
        }
        Collections.reverse(large);
        small.addAll(large);
        return small;
    }
}
```