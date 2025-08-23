[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3653\. XOR After Range Multiplication Queries I

Medium

You are given an integer array `nums` of length `n` and a 2D integer array `queries` of size `q`, where <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>, k<sub>i</sub>, v<sub>i</sub>]</code>.

For each query, you must apply the following operations in order:

*   Set <code>idx = l<sub>i</sub></code>.
*   While <code>idx <= r<sub>i</sub></code>:
    *   Update: <code>nums[idx] = (nums[idx] * v<sub>i</sub>) % (10<sup>9</sup> + 7)</code>
    *   Set <code>idx += k<sub>i</sub></code>.

Return the **bitwise XOR** of all elements in `nums` after processing all queries.

**Example 1:**

**Input:** nums = [1,1,1], queries = \[\[0,2,1,4]]

**Output:** 4

**Explanation:**

*   A single query `[0, 2, 1, 4]` multiplies every element from index 0 through index 2 by 4.
*   The array changes from `[1, 1, 1]` to `[4, 4, 4]`.
*   The XOR of all elements is `4 ^ 4 ^ 4 = 4`.

**Example 2:**

**Input:** nums = [2,3,1,5,4], queries = \[\[1,4,2,3],[0,2,1,2]]

**Output:** 31

**Explanation:**

*   The first query `[1, 4, 2, 3]` multiplies the elements at indices 1 and 3 by 3, transforming the array to `[2, 9, 1, 15, 4]`.
*   The second query `[0, 2, 1, 2]` multiplies the elements at indices 0, 1, and 2 by 2, resulting in `[4, 18, 2, 15, 4]`.
*   Finally, the XOR of all elements is `4 ^ 18 ^ 2 ^ 15 ^ 4 = 31`.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>3</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= q == queries.length <= 10<sup>3</sup></code>
*   <code>queries[i] = [l<sub>i</sub>, r<sub>i</sub>, k<sub>i</sub>, v<sub>i</sub>]</code>
*   <code>0 <= l<sub>i</sub> <= r<sub>i</sub> < n</code>
*   <code>1 <= k<sub>i</sub> <= n</code>
*   <code>1 <= v<sub>i</sub> <= 10<sup>5</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private static final int MOD = 1_000_000_007;

    private long modPow(long a, long e) {
        long res = 1;
        while (e > 0) {
            if ((e & 1) == 1) {
                res = (res * a) % MOD;
            }
            a = (a * a) % MOD;
            e >>= 1;
        }
        return res;
    }

    private long modInv(long a) {
        return modPow(a, MOD - 2L);
    }

    public int xorAfterQueries(int[] nums, int[][] queries) {
        int n = nums.length;
        int b = (int) Math.sqrt(n);
        // Store difference arrays for small k
        // map: k -> array of diff arrays (each residue class has diff array)
        Map<Integer, long[][]> small = new HashMap<>();
        for (int[] query : queries) {
            int l = query[0];
            int r = query[1];
            int k = query[2];
            int v = query[3];
            if (k > b) {
                // Process directly
                for (int i = l; i <= r; i += k) {
                    nums[i] = (int) (((long) nums[i] * v) % MOD);
                }
            } else {
                // Ensure storage
                small.putIfAbsent(k, new long[k][]);
                long[][] byResidue = small.get(k);
                int res = l % k;
                if (byResidue[res] == null) {
                    // number of elements with this residue
                    int len = (n - res + k - 1) / k;
                    // diff array
                    byResidue[res] = new long[len + 1];
                    Arrays.fill(byResidue[res], 1L);
                }

                long[] diff = byResidue[res];
                int jStart = (l - res) / k;
                int jEnd = (r - res) / k;

                diff[jStart] = (diff[jStart] * v) % MOD;
                if (jEnd + 1 < diff.length) {
                    diff[jEnd + 1] = (diff[jEnd + 1] * modInv(v)) % MOD;
                }
            }
        }
        // Apply small k modifications
        for (Map.Entry<Integer, long[][]> entry : small.entrySet()) {
            int k = entry.getKey();
            long[][] byResidue = entry.getValue();
            for (int res = 0; res < k; res++) {
                if (byResidue[res] == null) {
                    continue;
                }
                long[] diff = byResidue[res];
                long mul = 1;
                for (int j = 0; j < diff.length - 1; j++) {
                    mul = (mul * diff[j]) % MOD;
                    int idx = res + j * k;
                    if (idx < n) {
                        nums[idx] = (int) ((nums[idx] * mul) % MOD);
                    }
                }
            }
        }
        int ans = 0;
        for (int x : nums) {
            ans ^= x;
        }
        return ans;
    }
}
```