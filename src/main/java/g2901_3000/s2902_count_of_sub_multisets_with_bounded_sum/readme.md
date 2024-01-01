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
import java.util.List;

public class Solution {
    private static final int MOD = 1000000007;
    private static final int MAX = 20001;
    private static final IntMap INT_MAP = new IntMap();

    public int countSubMultisets(List<Integer> nums, int l, int r) {
        INT_MAP.clear();
        INT_MAP.add(0);
        int total = 0;
        for (int num : nums) {
            INT_MAP.add(num);
            total += num;
        }
        if (total < l) {
            return 0;
        }
        r = Math.min(r, total);
        final int[] cnt = new int[r + 1];
        cnt[0] = INT_MAP.map[0];
        int sum = 0;
        for (int i = 1; i < INT_MAP.size; i++) {
            final int val = INT_MAP.vals[i];
            final int count = INT_MAP.map[val];
            if (count > 0) {
                sum = Math.min(r, sum + val * count);
                update(cnt, val, count, sum);
            }
        }
        int res = 0;
        for (int i = l; i <= r; i++) {
            res = (res + cnt[i]) % MOD;
        }
        return res;
    }

    private static void update(final int[] cnt, final int n, final int count, final int sum) {
        if (count == 1) {
            for (int i = sum; i >= n; i--) {
                cnt[i] = (cnt[i] + cnt[i - n]) % MOD;
            }
        } else {
            for (int i = n; i <= sum; i++) {
                cnt[i] = (cnt[i] + cnt[i - n]) % MOD;
            }
            final int max = (count + 1) * n;
            for (int i = sum; i >= max; i--) {
                cnt[i] = (cnt[i] - cnt[i - max] + MOD) % MOD;
            }
        }
    }

    private static final class IntMap {
        final int[] map = new int[MAX];
        final int[] vals = new int[MAX];
        int size = 0;

        void add(int v) {
            if (map[v]++ == 0) {
                vals[size++] = v;
            }
        }

        void clear() {
            for (int i = 0; i < size; i++) {
                map[vals[i]] = 0;
            }
            size = 0;
        }
    }
}
```