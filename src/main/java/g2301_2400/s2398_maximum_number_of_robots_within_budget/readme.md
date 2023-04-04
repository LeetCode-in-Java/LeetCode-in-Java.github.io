[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2398\. Maximum Number of Robots Within Budget

Hard

You have `n` robots. You are given two **0-indexed** integer arrays, `chargeTimes` and `runningCosts`, both of length `n`. The <code>i<sup>th</sup></code> robot costs `chargeTimes[i]` units to charge and costs `runningCosts[i]` units to run. You are also given an integer `budget`.

The **total cost** of running `k` chosen robots is equal to `max(chargeTimes) + k * sum(runningCosts)`, where `max(chargeTimes)` is the largest charge cost among the `k` robots and `sum(runningCosts)` is the sum of running costs among the `k` robots.

Return _the **maximum** number of **consecutive** robots you can run such that the total cost **does not** exceed_ `budget`.

**Example 1:**

**Input:** chargeTimes = [3,6,1,3,4], runningCosts = [2,1,3,4,5], budget = 25

**Output:** 3

**Explanation:**

It is possible to run all individual and consecutive pairs of robots within budget.

To obtain answer 3, consider the first 3 robots. The total cost will be max(3,6,1) + 3 \* sum(2,1,3) = 6 + 3 \* 6 = 24 which is less than 25.

It can be shown that it is not possible to run more than 3 consecutive robots within budget, so we return 3. 

**Example 2:**

**Input:** chargeTimes = [11,12,19], runningCosts = [10,8,7], budget = 19

**Output:** 0

**Explanation:** No robot can be run that does not exceed the budget, so we return 0. 

**Constraints:**

*   `chargeTimes.length == runningCosts.length == n`
*   <code>1 <= n <= 5 * 10<sup>4</sup></code>
*   <code>1 <= chargeTimes[i], runningCosts[i] <= 10<sup>5</sup></code>
*   <code>1 <= budget <= 10<sup>15</sup></code>

## Solution

```java
public class Solution {

    // use sliding window to track the largest in a way that the sliding window only grows.
    //   then the maximum size is the size of the sliding window at the end.
    // if condition is met, we just grow the sliding window.
    // if condition is not met, we shift the sliding window with the same size to the next position.
    // e.g., if [0,3] is valid, next time we will try [0,4].
    //       if [0,3] is invalid, next time we will try [1,4],
    //         by adjusting the window to [1,3] first in the current round.
    public int maximumRobots(int[] chargeTimes, int[] runningCosts, long budget) {
        int n = chargeTimes.length;
        // [front, end).
        int[] deque = new int[n];
        int front = 0;
        int end = 0;
        long sum = 0;
        int left = 0;
        int right = 0;
        for (; right < n; ++right) {
            // add right into the sliding window, so the window becomes [left, right].
            // update sliding window max and window sum.
            while (end - front > 0 && chargeTimes[deque[end - 1]] <= chargeTimes[right]) {
                --end;
            }
            deque[end++] = right;
            sum += runningCosts[right];
            // if the condition is met in the window, do nothing,
            // so the next window size will become one larger.
            // if the condition is not met in the window, shrink one from the front,
            // so the next window size will stay the same.
            if (chargeTimes[deque[front]] + (right - left + 1) * sum > budget) {
                while (end - front > 0 && deque[front] <= left) {
                    ++front;
                }
                sum -= runningCosts[left];
                ++left;
            }
        }
        return right - left;
    }
}
```