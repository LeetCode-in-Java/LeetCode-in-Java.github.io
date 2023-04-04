[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 826\. Most Profit Assigning Work

Medium

You have `n` jobs and `m` workers. You are given three arrays: `difficulty`, `profit`, and `worker` where:

*   `difficulty[i]` and `profit[i]` are the difficulty and the profit of the <code>i<sup>th</sup></code> job, and
*   `worker[j]` is the ability of <code>j<sup>th</sup></code> worker (i.e., the <code>j<sup>th</sup></code> worker can only complete a job with difficulty at most `worker[j]`).

Every worker can be assigned **at most one job**, but one job can be **completed multiple times**.

*   For example, if three workers attempt the same job that pays `$1`, then the total profit will be `$3`. If a worker cannot complete any job, their profit is `$0`.

Return the maximum profit we can achieve after assigning the workers to the jobs.

**Example 1:**

**Input:** difficulty = [2,4,6,8,10], profit = [10,20,30,40,50], worker = [4,5,6,7]

**Output:** 100

**Explanation:** Workers are assigned jobs of difficulty [4,4,6,6] and they get a profit of [20,20,30,30] separately.

**Example 2:**

**Input:** difficulty = [85,47,57], profit = [24,66,99], worker = [40,25,25]

**Output:** 0

**Constraints:**

*   `n == difficulty.length`
*   `n == profit.length`
*   `m == worker.length`
*   <code>1 <= n, m <= 10<sup>4</sup></code>
*   <code>1 <= difficulty[i], profit[i], worker[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int maxProfitAssignment(int[] difficulty, int[] profit, int[] worker) {
        int n = 100000;
        int[] maxProfit = new int[n];

        for (int i = 0; i < difficulty.length; i++) {
            maxProfit[difficulty[i]] = Math.max(maxProfit[difficulty[i]], profit[i]);
        }

        for (int i = 1; i < n; i++) {
            maxProfit[i] = Math.max(maxProfit[i], maxProfit[i - 1]);
        }

        int sum = 0;
        for (int efficiency : worker) {
            sum += maxProfit[efficiency];
        }
        return sum;
    }
}
```