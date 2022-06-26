## 397\. Integer Replacement

Medium

Given a positive integer `n`, you can apply one of the following operations:

1.  If `n` is even, replace `n` with `n / 2`.
2.  If `n` is odd, replace `n` with either `n + 1` or `n - 1`.

Return _the minimum number of operations needed for `n` to become `1`_.

**Example 1:**

**Input:** n = 8

**Output:** 3

**Explanation:** 8 -> 4 -> 2 -> 1

**Example 2:**

**Input:** n = 7

**Output:** 4

**Explanation:** 7 -> 8 -> 4 -> 2 -> 1 or 7 -> 6 -> 3 -> 2 -> 1

**Example 3:**

**Input:** n = 4

**Output:** 2

**Constraints:**

*   <code>1 <= n <= 2<sup>31</sup> - 1</code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int integerReplacement(int n) {
        Map<Integer, Integer> dp = new HashMap<>();
        return solve(n, dp);
    }

    private int solve(int n, Map<Integer, Integer> dp) {
        if (n == 1) {
            return 0;
        }
        if (dp.containsKey(n)) {
            return dp.get(n);
        }
        int ans;
        if (n % 2 == 0) {
            ans = 1 + solve(n >>> 1, dp);
        } else {
            ans = 1 + Math.min(solve(n + 1, dp), solve(n - 1, dp));
        }
        dp.put(n, ans);
        return ans;
    }
}
```