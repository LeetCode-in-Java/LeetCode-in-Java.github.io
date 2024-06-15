[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3181\. Maximum Total Reward Using Operations II

Hard

You are given an integer array `rewardValues` of length `n`, representing the values of rewards.

Initially, your total reward `x` is 0, and all indices are **unmarked**. You are allowed to perform the following operation **any** number of times:

*   Choose an **unmarked** index `i` from the range `[0, n - 1]`.
*   If `rewardValues[i]` is **greater** than your current total reward `x`, then add `rewardValues[i]` to `x` (i.e., `x = x + rewardValues[i]`), and **mark** the index `i`.

Return an integer denoting the **maximum** _total reward_ you can collect by performing the operations optimally.

**Example 1:**

**Input:** rewardValues = [1,1,3,3]

**Output:** 4

**Explanation:**

During the operations, we can choose to mark the indices 0 and 2 in order, and the total reward will be 4, which is the maximum.

**Example 2:**

**Input:** rewardValues = [1,6,4,3,2]

**Output:** 11

**Explanation:**

Mark the indices 0, 2, and 1 in order. The total reward will then be 11, which is the maximum.

**Constraints:**

*   <code>1 <= rewardValues.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= rewardValues[i] <= 5 * 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int maxTotalReward(int[] rewardValues) {
        int max = rewardValues[0];
        int n = 0;
        for (int i = 1; i < rewardValues.length; i++) {
            max = Math.max(max, rewardValues[i]);
        }
        boolean[] vis = new boolean[max + 1];
        for (int i : rewardValues) {
            if (!vis[i]) {
                n++;
                vis[i] = true;
            }
        }
        int[] rew = new int[n];
        int j = 0;
        for (int i = 0; i <= max; i++) {
            if (vis[i]) {
                rew[j++] = i;
            }
        }
        return rew[n - 1] + getAns(rew, n - 1, rew[n - 1] - 1);
    }

    private int getAns(int[] rewards, int i, int validLimit) {
        int res = 0;
        int j = nextElemWithinLimits(rewards, i - 1, validLimit);
        for (; j >= 0; j--) {
            if (res >= rewards[j] + Math.min(validLimit - rewards[j], rewards[j] - 1)) {
                break;
            }
            res =
                    Math.max(
                            res,
                            rewards[j]
                                    + getAns(
                                            rewards,
                                            j,
                                            Math.min(validLimit - rewards[j], rewards[j] - 1)));
        }
        return res;
    }

    private int nextElemWithinLimits(int[] rewards, int h, int k) {
        int l = 0;
        int resInd = -1;
        while (l <= h) {
            int m = (l + h) / 2;
            if (rewards[m] <= k) {
                resInd = m;
                l = m + 1;
            } else {
                h = m - 1;
            }
        }
        return resInd;
    }
}
```