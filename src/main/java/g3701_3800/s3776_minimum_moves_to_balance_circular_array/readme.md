[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3776\. Minimum Moves to Balance Circular Array

Medium

You are given a **circular** array `balance` of length `n`, where `balance[i]` is the net balance of person `i`.

In one move, a person can transfer **exactly** 1 unit of balance to either their left or right neighbor.

Return the **minimum** number of moves required so that every person has a **non-negative** balance. If it is impossible, return `-1`.

**Note**: You are guaranteed that **at most** 1 index has a **negative** balance initially.

**Example 1:**

**Input:** balance = [5,1,-4]

**Output:** 4

**Explanation:**

One optimal sequence of moves is:

*   Move 1 unit from `i = 1` to `i = 2`, resulting in `balance = [5, 0, -3]`
*   Move 1 unit from `i = 0` to `i = 2`, resulting in `balance = [4, 0, -2]`
*   Move 1 unit from `i = 0` to `i = 2`, resulting in `balance = [3, 0, -1]`
*   Move 1 unit from `i = 0` to `i = 2`, resulting in `balance = [2, 0, 0]`

Thus, the minimum number of moves required is 4.

**Example 2:**

**Input:** balance = [1,2,-5,2]

**Output:** 6

**Explanation:**

One optimal sequence of moves is:

*   Move 1 unit from `i = 1` to `i = 2`, resulting in `balance = [1, 1, -4, 2]`
*   Move 1 unit from `i = 1` to `i = 2`, resulting in `balance = [1, 0, -3, 2]`
*   Move 1 unit from `i = 3` to `i = 2`, resulting in `balance = [1, 0, -2, 1]`
*   Move 1 unit from `i = 3` to `i = 2`, resulting in `balance = [1, 0, -1, 0]`
*   Move 1 unit from `i = 0` to `i = 1`, resulting in `balance = [0, 1, -1, 0]`
*   Move 1 unit from `i = 1` to `i = 2`, resulting in `balance = [0, 0, 0, 0]`

Thus, the minimum number of moves required is 6.

**Example 3:**

**Input:** balance = [-3,2]

**Output:** \-1

**Explanation:**

It is impossible to make all balances non-negative for `balance = [-3, 2]`, so the answer is -1.

**Constraints:**

*   <code>1 <= n == balance.length <= 10<sup>5</sup></code>
*   <code>-10<sup>9</sup> <= balance[i] <= 10<sup>9</sup></code>
*   There is at most one negative value in `balance` initially.

## Solution

```java
public class Solution {
    public long minMoves(int[] balance) {
        int n = balance.length;
        int j = -1;
        long total = 0;
        long res = 0;
        for (int i = 0; i < n; i++) {
            if (balance[i] < 0) {
                j = i;
            }
            total += balance[i];
        }
        if (j == -1) {
            return 0;
        }
        if (total < 0) {
            return -1;
        }
        for (int d = 1; balance[j] < 0; ++d) {
            long storage = balance[(j + d) % n] + (long) balance[(j - d % n + n) % n];
            res += Math.min(-balance[j], (int) storage) * d;
            balance[j] += (int) storage;
        }
        return res;
    }
}
```