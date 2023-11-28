[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2818\. Apply Operations to Maximize Score

Hard

You are given an array `nums` of `n` positive integers and an integer `k`.

Initially, you start with a score of `1`. You have to maximize your score by applying the following operation at most `k` times:

*   Choose any **non-empty** subarray `nums[l, ..., r]` that you haven't chosen previously.
*   Choose an element `x` of `nums[l, ..., r]` with the highest **prime score**. If multiple such elements exist, choose the one with the smallest index.
*   Multiply your score by `x`.

Here, `nums[l, ..., r]` denotes the subarray of `nums` starting at index `l` and ending at the index `r`, both ends being inclusive.

The **prime score** of an integer `x` is equal to the number of distinct prime factors of `x`. For example, the prime score of `300` is `3` since `300 = 2 * 2 * 3 * 5 * 5`.

Return _the **maximum possible score** after applying at most_ `k` _operations_.

Since the answer may be large, return it modulo <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [8,3,9,3,8], k = 2

**Output:** 81

**Explanation:**

To get a score of 81, we can apply the following operations:

- Choose subarray nums[2, ..., 2]. nums[2] is the only element in this subarray. Hence, we multiply the score by nums[2]. The score becomes 1 \* 9 = 9.

- Choose subarray nums[2, ..., 3]. Both nums[2] and nums[3] have a prime score of 1, but nums[2] has the smaller index. Hence, we multiply the score by nums[2]. The score becomes 9 \* 9 = 81.

It can be proven that 81 is the highest score one can obtain.

**Example 2:**

**Input:** nums = [19,12,14,6,10,18], k = 3

**Output:** 4788

**Explanation:**

To get a score of 4788, we can apply the following operations:

- Choose subarray nums[0, ..., 0]. nums[0] is the only element in this subarray. Hence, we multiply the score by nums[0]. The score becomes 1 \* 19 = 19.

- Choose subarray nums[5, ..., 5]. nums[5] is the only element in this subarray. Hence, we multiply the score by nums[5]. The score becomes 19 \* 18 = 342.

- Choose subarray nums[2, ..., 3]. Both nums[2] and nums[3] have a prime score of 2, but nums[2] has the smaller index. Hence, we multipy the score by nums[2]. The score becomes 342 \* 14 = 4788. It can be proven that 4788 is the highest score one can obtain. 

**Constraints:**

*   <code>1 <= nums.length == n <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= k <= min(n * (n + 1) / 2, 10<sup>9</sup>)</code>

## Solution

```java
import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Deque;
import java.util.List;
import java.util.PriorityQueue;

public class Solution {
    private static final int N = 100000;
    private static final int[] PRIME_SCORES = computePrimeScores();
    private static final int MOD = 1_000_000_000 + 7;

    public int maximumScore(List<Integer> nums, int k) {
        // count strictly using nums.get(i) as the selected num
        int[] dp = new int[nums.size()];
        // [val, index]
        PriorityQueue<int[]> pq = new PriorityQueue<>((o1, o2) -> Integer.compare(o2[0], o1[0]));
        Deque<int[]> monoStack = new ArrayDeque<>();
        Arrays.fill(dp, 1);
        for (int i = 0; i <= nums.size(); i++) {
            int score = Integer.MAX_VALUE;
            if (i < nums.size()) {
                score = PRIME_SCORES[nums.get(i)];
            }
            // when an element is poped, its right bound is confirmed: (i - left + 1) * (right - i +
            // 1)
            while (!monoStack.isEmpty() && monoStack.peekFirst()[0] < score) {
                int popIndex = monoStack.pollFirst()[1];
                int actualRightIndexOfPopedElement = i - 1;
                dp[popIndex] *= (actualRightIndexOfPopedElement - popIndex + 1);
            }
            // when an element is pushed, its left bound is confirmed: (i - left + 1) * (right - i +
            // 1)
            if (i < nums.size()) {
                int peekIndex = monoStack.isEmpty() ? -1 : monoStack.peekFirst()[1];
                int actualLeftIndexOfCurrentElement = peekIndex + 1;
                dp[i] *= (i - actualLeftIndexOfCurrentElement + 1);
                monoStack.offerFirst(new int[] {score, i});
                pq.offer(new int[] {nums.get(i), i});
            }
        }
        long result = 1;
        while (k > 0) {
            int[] pair = pq.poll();
            int val = pair[0];
            int index = pair[1];
            int times = Math.min(k, dp[index]);
            long power = pow(val, times);
            result *= power;
            result %= MOD;
            k -= times;
        }
        return (int) result;
    }

    private static int[] computePrimeScores() {
        int[] primeCnt = new int[N + 1];
        for (int i = 2; i <= N; i++) {
            if (primeCnt[i] == 0) {
                for (int j = i; j <= N; j += i) {
                    primeCnt[j]++;
                }
            }
        }
        return primeCnt;
    }

    private long pow(long val, int times) {
        if (times == 1) {
            return val % MOD;
        }
        long subProblemRes = pow(val, times / 2);
        long third = 1L;
        if (times % 2 == 1) {
            third = val;
        }
        return subProblemRes * subProblemRes % MOD * third % MOD;
    }
}
```