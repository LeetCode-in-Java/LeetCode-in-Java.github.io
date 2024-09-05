[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3266\. Final Array State After K Multiplication Operations II

Hard

You are given an integer array `nums`, an integer `k`, and an integer `multiplier`.

You need to perform `k` operations on `nums`. In each operation:

*   Find the **minimum** value `x` in `nums`. If there are multiple occurrences of the minimum value, select the one that appears **first**.
*   Replace the selected minimum value `x` with `x * multiplier`.

After the `k` operations, apply **modulo** <code>10<sup>9</sup> + 7</code> to every value in `nums`.

Return an integer array denoting the _final state_ of `nums` after performing all `k` operations and then applying the modulo.

**Example 1:**

**Input:** nums = [2,1,3,5,6], k = 5, multiplier = 2

**Output:** [8,4,6,5,6]

**Explanation:**

| Operation               | Result           |
|-------------------------|------------------|
| After operation 1       | [2, 2, 3, 5, 6]  |
| After operation 2       | [4, 2, 3, 5, 6]  |
| After operation 3       | [4, 4, 3, 5, 6]  |
| After operation 4       | [4, 4, 6, 5, 6]  |
| After operation 5       | [8, 4, 6, 5, 6]  |
| After applying modulo   | [8, 4, 6, 5, 6]  |

**Example 2:**

**Input:** nums = [100000,2000], k = 2, multiplier = 1000000

**Output:** [999999307,999999993]

**Explanation:**

| Operation               | Result               |
|-------------------------|----------------------|
| After operation 1       | [100000, 2000000000] |
| After operation 2       | [100000000000, 2000000000] |
| After applying modulo   | [999999307, 999999993] |

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   <code>1 <= k <= 10<sup>9</sup></code>
*   <code>1 <= multiplier <= 10<sup>6</sup></code>

## Solution

```java
import java.util.Arrays;
import java.util.PriorityQueue;

public class Solution {
    private static final int MOD = 1_000_000_007;

    public int[] getFinalState(int[] nums, int k, int multiplier) {
        if (multiplier == 1) {
            return nums;
        }
        int n = nums.length;
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }
        long[] a = new long[n];
        int left = k;
        boolean shouldExit = false;
        for (int i = 0; i < n && !shouldExit; i++) {
            long x = nums[i];
            while (x < mx) {
                x *= multiplier;
                if (--left < 0) {
                    shouldExit = true;
                    break;
                }
            }
            a[i] = x;
        }
        if (left < 0) {
            PriorityQueue<long[]> pq =
                    new PriorityQueue<>(
                            (p, q) ->
                                    p[0] != q[0]
                                            ? Long.compare(p[0], q[0])
                                            : Long.compare(p[1], q[1]));
            for (int i = 0; i < n; i++) {
                pq.offer(new long[] {nums[i], i});
            }
            while (k-- > 0) {
                long[] p = pq.poll();
                p[0] *= multiplier;
                pq.offer(p);
            }
            while (!pq.isEmpty()) {
                long[] p = pq.poll();
                nums[(int) p[1]] = (int) (p[0] % MOD);
            }
            return nums;
        }

        Integer[] ids = new Integer[n];
        Arrays.setAll(ids, i -> i);
        Arrays.sort(ids, (i, j) -> Long.compare(a[i], a[j]));
        k = left;
        long pow1 = pow(multiplier, k / n);
        long pow2 = pow1 * multiplier % MOD;
        for (int i = 0; i < n; i++) {
            int j = ids[i];
            nums[j] = (int) (a[j] % MOD * (i < k % n ? pow2 : pow1) % MOD);
        }
        return nums;
    }

    private long pow(long x, int n) {
        long res = 1;
        for (; n > 0; n /= 2) {
            if (n % 2 > 0) {
                res = res * x % MOD;
            }
            x = x * x % MOD;
        }
        return res;
    }
}
```