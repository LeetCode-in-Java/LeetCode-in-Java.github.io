## 1923\. Longest Common Subpath

Hard

There is a country of `n` cities numbered from `0` to `n - 1`. In this country, there is a road connecting **every pair** of cities.

There are `m` friends numbered from `0` to `m - 1` who are traveling through the country. Each one of them will take a path consisting of some cities. Each path is represented by an integer array that contains the visited cities in order. The path may contain a city **more than once**, but the same city will not be listed consecutively.

Given an integer `n` and a 2D integer array `paths` where `paths[i]` is an integer array representing the path of the <code>i<sup>th</sup></code> friend, return _the length of the **longest common subpath** that is shared by **every** friend's path, or_ `0` _if there is no common subpath at all_.

A **subpath** of a path is a contiguous sequence of cities within that path.

**Example 1:**

**Input:** n = 5, paths = [[0,1,2,3,4], 
                           [2,3,4], 
                           [4,0,1,2,3]]

**Output:** 2

**Explanation:** The longest common subpath is [2,3].

**Example 2:**

**Input:** n = 3, paths = [[0],[1],[2]]

**Output:** 0

**Explanation:** There is no common subpath shared by the three paths.

**Example 3:**

**Input:** n = 5, paths = [[0,1,2,3,4], 
                           [4,3,2,1,0]]

**Output:** 1

**Explanation:** The possible longest common subpaths are [0], [1], [2], [3], and [4]. All have a length of 1.

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>
*   `m == paths.length`
*   <code>2 <= m <= 10<sup>5</sup></code>
*   <code>sum(paths[i].length) <= 10<sup>5</sup></code>
*   `0 <= paths[i][j] < n`
*   The same city is not listed multiple times consecutively in `paths[i]`.

## Solution

```java
import java.util.HashSet;

@SuppressWarnings("java:S1172")
public class Solution {
    private static final long BASE = 100001;
    private static final long MOD = (long) (Math.pow(10, 11) + 7);
    private long[] pow;

    public int longestCommonSubpath(int n, int[][] paths) {
        int res = 0;
        int min = Integer.MAX_VALUE;
        for (int[] path : paths) {
            min = Math.min(min, path.length);
        }
        pow = new long[min + 1];
        pow[0]++;
        for (int i = 1; i <= min; i++) {
            pow[i] = (pow[i - 1] * BASE) % MOD;
        }
        int st = 1;
        int end = min;
        int mid = (st + end) / 2;
        while (st <= end) {
            if (commonSubstring(paths, mid)) {
                res = mid;
                st = mid + 1;
            } else {
                end = mid - 1;
            }
            mid = (st + end) / 2;
        }
        return res;
    }

    private boolean commonSubstring(int[][] paths, int l) {
        HashSet<Long> set = rollingHash(paths[0], l);
        for (int i = 1, n = paths.length; i < n; i++) {
            set.retainAll(rollingHash(paths[i], l));
            if (set.isEmpty()) {
                return false;
            }
        }
        return true;
    }

    private HashSet<Long> rollingHash(int[] a, int l) {
        HashSet<Long> set = new HashSet<>();
        long hash = 0;
        for (int i = 0; i < l; i++) {
            hash = (hash * BASE + a[i]) % MOD;
        }
        set.add(hash);
        for (int n = a.length, curr = l, prev = 0; curr < n; prev++, curr++) {
            hash = (((hash * BASE) % MOD - (a[prev] * pow[l]) % MOD + a[curr]) + MOD) % MOD;
            set.add(hash);
        }
        return set;
    }
}
```