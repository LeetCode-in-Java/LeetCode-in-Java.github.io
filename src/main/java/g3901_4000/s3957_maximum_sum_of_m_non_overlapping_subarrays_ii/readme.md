[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3957\. Maximum Sum of M Non-Overlapping Subarrays II

Hard

You are given an integer array `nums` of length `n`, and three integers `m`, `l`, and `r`.

Your task is to select **at least** one and **at most** `m` **non-overlapping non-empty subarrays** from `nums` such that:

*   Each selected **subarray** has a length between `[l, r]` (inclusive).
*   The total sum of all selected **subarrays** is **maximized**.

Return the **maximum** total sum you can achieve.

**Example 1:**

**Input:** nums = [4,1,-5,2], m = 2, l = 1, r = 3

**Output:** 7

**Explanation:**

One optimal strategy is to:

*   Select the subarray `[4, 1]` with sum `4 + 1 = 5` and the subarray `[2]` with sum 2. Both subarrays have length between `[l, r]`.
*   The total sum of these subarrays is `5 + 2 = 7`, which is the maximum achievable sum with at most `m = 2` subarrays.

**Example 2:**

**Input:** nums = [1,0,3,4], m = 2, l = 1, r = 2

**Output:** 8

**Explanation:**

One optimal strategy is to:

*   Select the subarray `[1]` with sum `1` and the subarray `[3, 4]` with sum `3 + 4 = 7`. Both subarrays have length between `[l, r]`.
*   The total sum of these subarrays is `1 + 7 = 8`, which is the maximum achievable sum with at most `m = 2` subarrays.

**Example 3:**

**Input:** nums = [-1,7,-4], m = 1, l = 2, r = 3

**Output:** 6

**Explanation:**

*   Select the subarray `[-1, 7]` from `nums` which has length between `[l, r]`.
*   The total sum of this subarray is `-1 + 7 = 6`, which is the maximum achievable sum with at most `m = 1` subarray.

**Example 4:**

**Input:** nums = [-3,-4,-1], m = 2, l = 1, r = 2

**Output:** \-1

**Explanation:**

*   All subarrays of `nums` have negative sums. The optimal strategy is to select the subarray `[-1]`, which has length between `[l, r]`.
*   The total sum of this subarray is -1, which is the maximum achievable sum with at most `m = 2` subarrays.

**Constraints:**

*   <code>1 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   `1 <= m <= n`
*   `1 <= l <= r <= n`

## Solution

```java
public class Solution {
    private long[] pre;
    private long[] val;
    private int[] cnt;
    private int[] dq;
    private int n;
    private int l;
    private int r;

    public long maximumSum(int[] nums, int m, int l, int r) {
        n = nums.length;
        this.l = l;
        this.r = r;
        pre = new long[n + 1];
        for (int i = 0; i < n; i++) {
            pre[i + 1] = pre[i] + nums[i];
        }
        val = new long[n + 1];
        cnt = new int[n + 1];
        dq = new int[n + 2];
        long lo = 0;
        long hi = 20000000000L;
        long ansLambda = hi;
        solve(0);
        if (cnt[n] <= m) {
            return val[n];
        }
        while (lo <= hi) {
            long mid = lo + (hi - lo) / 2;
            solve(mid);
            if (cnt[n] <= m) {
                ansLambda = mid;
                hi = mid - 1;
            } else {
                lo = mid + 1;
            }
        }
        solve(ansLambda);
        return val[n] + ansLambda * m;
    }

    private void solve(long lambda) {
        final long neg = Long.MIN_VALUE / 4;
        val[0] = neg;
        cnt[0] = 0;
        int head = 0;
        int tail = 0;
        for (int i = 1; i <= n; i++) {
            int j = i - l;
            if (j >= 0) {
                tail = addCandidate(j, head, tail);
            }
            head = removeExpiredCandidates(head, tail, i - r);
            long bestV = val[i - 1];
            int bestC = cnt[i - 1];
            if (tail > head) {
                int b = dq[head];
                long cand = normalizedValue(b) - pre[b] + pre[i] - lambda;
                int candC = normalizedCount(b) + 1;
                if (isBetter(cand, candC, bestV, bestC)) {
                    bestV = cand;
                    bestC = candC;
                }
            }
            val[i] = bestV;
            cnt[i] = bestC;
        }
    }

    private int addCandidate(int j, int head, int tail) {
        long key = normalizedValue(j) - pre[j];
        int candidateCount = normalizedCount(j);
        while (tail > head) {
            int t = dq[tail - 1];
            long tailKey = normalizedValue(t) - pre[t];
            int tailCount = normalizedCount(t);
            if (tailKey < key || (tailKey == key && tailCount >= candidateCount)) {
                tail--;
            } else {
                break;
            }
        }
        dq[tail++] = j;
        return tail;
    }

    private int removeExpiredCandidates(int head, int tail, int minIndex) {
        while (tail > head && dq[head] < minIndex) {
            head++;
        }
        return head;
    }

    private long normalizedValue(int index) {
        return val[index] > 0 ? val[index] : 0;
    }

    private int normalizedCount(int index) {
        return val[index] > 0 ? cnt[index] : 0;
    }

    private boolean isBetter(
            long candidateValue, int candidateCount, long bestValue, int bestCount) {
        return candidateValue > bestValue
                || (candidateValue == bestValue && candidateCount < bestCount);
    }
}
```