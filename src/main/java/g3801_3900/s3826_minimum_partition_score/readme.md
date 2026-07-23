[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3826\. Minimum Partition Score

Hard

You are given an integer array `nums` and an integer `k`.

Your task is to partition `nums` into **exactly** `k` non-empty subarrays and return an integer denoting the **minimum possible score** among all valid partitions.

The **score** of a partition is the **sum** of the **values** of all its subarrays.

The **value** of a subarray is defined as `sumArr * (sumArr + 1) / 2`, where `sumArr` is the sum of its elements.

**Example 1:**

**Input:** nums = [5,1,2,1], k = 2

**Output:** 25

**Explanation:**

*   We must partition the array into `k = 2` subarrays. One optimal partition is `[5]` and `[1, 2, 1]`.
*   The first subarray has `sumArr = 5` and `value = 5 × 6 / 2 = 15`.
*   The second subarray has `sumArr = 1 + 2 + 1 = 4` and `value = 4 × 5 / 2 = 10`.
*   The score of this partition is `15 + 10 = 25`, which is the minimum possible score.

**Example 2:**

**Input:** nums = [1,2,3,4], k = 1

**Output:** 55

**Explanation:**

*   Since we must partition the array into `k = 1` subarray, all elements belong to the same subarray: `[1, 2, 3, 4]`.
*   This subarray has `sumArr = 1 + 2 + 3 + 4 = 10` and `value = 10 × 11 / 2 = 55`.
*   The score of this partition is 55, which is the minimum possible score.

**Example 3:**

**Input:** nums = [1,1,1], k = 3

**Output:** 3

**Explanation:**

*   We must partition the array into `k = 3` subarrays. The only valid partition is `[1], [1], [1]`.
*   Each subarray has `sumArr = 1` and `value = 1 × 2 / 2 = 1`.
*   The score of this partition is `1 + 1 + 1 = 3`, which is the minimum possible score.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 10<sup>4</sup></code>
*   `1 <= k <= nums.length`

## Solution

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Solution {
    private long[] prefix;

    private boolean isRedundant(Line l1, Line l2, Line l3) {
        return (double) (l3.intercept - l1.intercept) * (l1.slope - l2.slope)
                <= (double) (l2.intercept - l1.intercept) * (l1.slope - l3.slope);
    }

    private Pair solveWithPenalty(long lambda) {
        Deque<Line> dq = new ArrayDeque<>();
        dq.addLast(new Line(0, 0, 0));
        long di = 0;
        int pi = 0;
        for (int i = 1; i < prefix.length; i++) {
            long x = prefix[i];
            // query
            while (dq.size() > 1) {
                Line first = dq.pollFirst();
                Line second = dq.peekFirst();
                if (first.eval(x) <= second.eval(x)) {
                    dq.addFirst(first);
                    break;
                }
            }
            Line best = dq.peekFirst();
            di = x * x + x + 2 * lambda + best.eval(x);
            pi = best.partition + 1;
            Line newLine = new Line(-2 * x, di + x * x - x, pi);
            while (dq.size() > 1) {
                Line last = dq.pollLast();
                Line secondLast = dq.peekLast();
                if (!isRedundant(secondLast, last, newLine)) {
                    dq.addLast(last);
                    break;
                }
            }
            dq.addLast(newLine);
        }
        return new Pair(di, pi);
    }

    public long minPartitionScore(int[] nums, int k) {
        int n = nums.length;
        prefix = new long[n + 1];
        for (int i = 1; i <= n; i++) {
            prefix[i] = prefix[i - 1] + nums[i - 1];
        }
        long start = 0;
        long end = (long) 1e15;
        long bestLambda = 0;
        while (start <= end) {
            long mid = start + (end - start) / 2;
            Pair res = solveWithPenalty(mid);
            if (res.partition <= k) {
                bestLambda = mid;
                end = mid - 1;
            } else {
                start = mid + 1;
            }
        }
        Pair ans = solveWithPenalty(bestLambda);
        return (ans.score - 2L * k * bestLambda) / 2;
    }

    private static class Line {
        long slope;
        long intercept;
        int partition;

        Line(long slope, long intercept, int partition) {
            this.slope = slope;
            this.intercept = intercept;
            this.partition = partition;
        }

        long eval(long x) {
            return slope * x + intercept;
        }
    }

    private static class Pair {
        long score;
        int partition;

        Pair(long score, int partition) {
            this.score = score;
            this.partition = partition;
        }
    }
}
```