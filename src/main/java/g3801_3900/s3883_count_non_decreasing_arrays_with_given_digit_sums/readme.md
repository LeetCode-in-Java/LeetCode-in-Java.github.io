[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3883\. Count Non Decreasing Arrays With Given Digit Sums

Hard

You are given an integer array `digitSum` of length `n`.

An array `arr` of length `n` is considered **valid** if:

*   `0 <= arr[i] <= 5000`
*   it is **non-decreasing**.
*   the **sum of the digits** of `arr[i]` **equals** `digitSum[i]`.

Return an integer denoting the number of **distinct valid arrays**. Since the answer may be large, return it modulo <code>10<sup>9</sup> + 7</code>.

An array is said to be **non-decreasing** if each element is greater than or equal to the previous element, if it exists.

**Example 1:**

**Input:** digitSum = [25,1]

**Output:** 6

**Explanation:**

Numbers whose sum of digits is 25 are 799, 889, 898, 979, 988, and 997.

The only number whose sum of digits is 1 that can appear after these values while keeping the array non-decreasing is 1000.

Thus, the valid arrays are `[799, 1000]`, `[889, 1000]`, `[898, 1000]`, `[979, 1000]`, `[988, 1000]`, and `[997, 1000]`.

Hence, the answer is 6.

**Example 2:**

**Input:** digitSum = [1]

**Output:** 4

**Explanation:**

The valid arrays are `[1]`, `[10]`, `[100]`, and `[1000]`.

Thus, the answer is 4.

**Example 3:**

**Input:** digitSum = [2,49,23]

**Output:** 0

**Explanation:**

There is no integer in the range [0, 5000] whose sum of digits is 49. Thus, the answer is 0.

**Constraints:**

*   `1 <= digitSum.length <= 1000`
*   `0 <= digitSum[i] <= 50`

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;

@SuppressWarnings("unchecked")
public class Solution {
    private static final int M = 1000000007;

    private int s(int x) {
        int r = 0;
        while (x > 0) {
            r += x % 10;
            x /= 10;
        }
        return r;
    }

    public int countArrays(int[] d) {
        ArrayList<Integer>[] g = buildGroups();
        if (g[d[0]].isEmpty()) {
            return 0;
        }
        long[] dp = createInitialDp(g[d[0]].size());
        for (int i = 1; i < d.length; i++) {
            dp = transition(dp, g[d[i - 1]], g[d[i]]);
            if (dp.length == 0) {
                return 0;
            }
        }
        return sum(dp);
    }

    private ArrayList<Integer>[] buildGroups() {
        ArrayList<Integer>[] g = new ArrayList[51];
        for (int i = 0; i <= 50; i++) {
            g[i] = new ArrayList<>();
        }
        for (int i = 0; i <= 5000; i++) {
            g[s(i)].add(i);
        }
        return g;
    }

    private long[] createInitialDp(int size) {
        long[] dp = new long[size];
        Arrays.fill(dp, 1);
        return dp;
    }

    private long[] transition(long[] dp, ArrayList<Integer> previous, ArrayList<Integer> current) {
        if (current.isEmpty()) {
            return new long[0];
        }
        long[] prefix = buildPrefixSums(dp);
        long[] next = new long[current.size()];
        int k = 0;
        for (int j = 0; j < current.size(); j++) {
            while (k < previous.size() && previous.get(k) <= current.get(j)) {
                k++;
            }
            if (k > 0) {
                next[j] = prefix[k - 1];
            }
        }
        return next;
    }

    private long[] buildPrefixSums(long[] values) {
        long[] prefix = new long[values.length];
        prefix[0] = values[0];
        for (int i = 1; i < values.length; i++) {
            prefix[i] = (prefix[i - 1] + values[i]) % M;
        }
        return prefix;
    }

    private int sum(long[] values) {
        long result = 0;
        for (long value : values) {
            result = (result + value) % M;
        }
        return (int) result;
    }
}
```