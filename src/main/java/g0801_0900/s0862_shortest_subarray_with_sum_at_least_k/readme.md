[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 862\. Shortest Subarray with Sum at Least K

Hard

Given an integer array `nums` and an integer `k`, return _the length of the shortest non-empty **subarray** of_ `nums` _with a sum of at least_ `k`. If there is no such **subarray**, return `-1`.

A **subarray** is a **contiguous** part of an array.

**Example 1:**

**Input:** nums = [1], k = 1

**Output:** 1

**Example 2:**

**Input:** nums = [1,2], k = 4

**Output:** -1

**Example 3:**

**Input:** nums = [2,-1,2], k = 3

**Output:** 3

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>-10<sup>5</sup> <= nums[i] <= 10<sup>5</sup></code>
*   <code>1 <= k <= 10<sup>9</sup></code>

## Solution

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Solution {
    public int shortestSubarray(int[] nums, int k) {
        Deque<long[]> dq = new ArrayDeque<>();
        dq.offer(new long[] {-1, 0});
        int i = 0;
        long curSum = 0;
        int res = Integer.MAX_VALUE;
        while (i < nums.length) {
            curSum += nums[i];
            while (!dq.isEmpty() && dq.peekFirst()[1] <= curSum - k) {
                res = Math.min(res, (int) (i - dq.pollFirst()[0]));
            }
            while (!dq.isEmpty() && dq.peekLast()[1] >= curSum) {
                dq.pollLast();
            }
            dq.offerLast(new long[] {i, curSum});
            i++;
        }
        return res == Integer.MAX_VALUE ? -1 : res;
    }
}
```