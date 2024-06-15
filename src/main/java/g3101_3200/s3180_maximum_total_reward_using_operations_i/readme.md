[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3180\. Maximum Total Reward Using Operations I

Medium

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

*   `1 <= rewardValues.length <= 2000`
*   `1 <= rewardValues[i] <= 2000`

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    private int[] sortedSet(int[] values) {
        int max = 0;
        for (int x : values) {
            if (x > max) {
                max = x;
            }
        }
        boolean[] set = new boolean[max + 1];
        int n = 0;
        for (int x : values) {
            if (!set[x]) {
                set[x] = true;
                n++;
            }
        }
        int[] result = new int[n];
        for (int x = max; x > 0; x--) {
            if (set[x]) {
                result[--n] = x;
            }
        }
        return result;
    }

    public int maxTotalReward(int[] rewardValues) {
        rewardValues = sortedSet(rewardValues);
        int n = rewardValues.length;
        int max = rewardValues[n - 1];
        boolean[] isSumPossible = new boolean[max];
        isSumPossible[0] = true;
        int maxSum = 0;
        int last = 1;
        for (int sum = rewardValues[0]; sum < max; sum++) {
            while (last < n && rewardValues[last] <= sum) {
                last++;
            }
            int s2 = sum / 2;
            for (int i = last - 1; i >= 0; i--) {
                int x = rewardValues[i];
                if (x <= s2) {
                    break;
                }
                if (isSumPossible[sum - x]) {
                    isSumPossible[sum] = true;
                    maxSum = sum;
                    break;
                }
            }
        }
        return maxSum + max;
    }
}
```