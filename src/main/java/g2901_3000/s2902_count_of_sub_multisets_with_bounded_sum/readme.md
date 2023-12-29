[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2902\. Count of Sub-Multisets With Bounded Sum

Hard

You are given a **0-indexed** array `nums` of non-negative integers, and two integers `l` and `r`.

Return _the **count of sub-multisets** within_ `nums` _where the sum of elements in each subset falls within the inclusive range of_ `[l, r]`.

Since the answer may be large, return it modulo <code>10<sup>9</sup> + 7</code>.

A **sub-multiset** is an **unordered** collection of elements of the array in which a given value `x` can occur `0, 1, ..., occ[x]` times, where `occ[x]` is the number of occurrences of `x` in the array.

**Note** that:

*   Two **sub-multisets** are the same if sorting both sub-multisets results in identical multisets.
*   The sum of an **empty** multiset is `0`.

**Example 1:**

**Input:** nums = [1,2,2,3], l = 6, r = 6

**Output:** 1

**Explanation:** The only subset of nums that has a sum of 6 is {1, 2, 3}.

**Example 2:**

**Input:** nums = [2,1,4,2,7], l = 1, r = 5

**Output:** 7

**Explanation:** The subsets of nums that have a sum within the range [1, 5] are {1}, {2}, {4}, {2, 2}, {1, 2}, {1, 4}, and {1, 2, 2}.

**Example 3:**

**Input:** nums = [1,2,1,3,5,2], l = 3, r = 5

**Output:** 9

**Explanation:** The subsets of nums that have a sum within the range [3, 5] are {3}, {5}, {1, 2}, {1, 3}, {2, 2}, {2, 3}, {1, 1, 2}, {1, 1, 3}, and {1, 2, 2}.

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>0 <= nums[i] <= 2 * 10<sup>4</sup></code>
*   Sum of `nums` does not exceed <code>2 * 10<sup>4</sup></code>.
*   <code>0 <= l <= r <= 2 * 10<sup>4</sup></code>

## Solution

```java
import java.util.ArrayList;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;

public class Solution {
    private static final int MOD = (int) 1e9 + 7;
    private HashMap<Integer, Integer> map;
    private int[][] dp;

    private int solve(List<Integer> al, int l, int r, int index, int sum) {
        if (sum > r) {
            return 0;
        }
        long ans = 0;
        if (index >= al.size()) {
            return (int) ans;
        }
        if (dp[index][sum] != -1) {
            return dp[index][sum];
        }
        int cur = al.get(index);
        int count = map.get(cur);
        for (int i = 0; i <= count; i++) {
            int curSum = sum + cur * i;
            if (curSum > r) {
                break;
            }
            ans = ans + solve(al, l, r, index + 1, curSum);
            if (i != 0 && curSum >= l) {
                ans = ans + 1;
            }
            ans = ans % MOD;
        }
        dp[index][sum] = (int) ans;
        return (int) ans;
    }

    public int countSubMultisets(List<Integer> nums, int l, int r) {
        map = new HashMap<>();
        List<Integer> al = new ArrayList<>();
        for (int cur : nums) {
            int count = map.getOrDefault(cur, 0) + 1;
            map.put(cur, count);
            if (count == 1) {
                al.add(cur);
            }
        }
        int n = al.size();
        dp = new int[n][r + 1];
        for (int i = 0; i < dp.length; i++) {
            for (int j = 0; j < dp[0].length; j++) {
                dp[i][j] = -1;
            }
        }
        Collections.sort(al);
        int ans = solve(al, l, r, 0, 0);
        if (l == 0) {
            ans = ans + 1;
        }
        ans = ans % MOD;
        return ans;
    }
}
```