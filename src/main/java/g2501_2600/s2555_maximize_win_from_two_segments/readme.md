[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2555\. Maximize Win From Two Segments

Medium

There are some prizes on the **X-axis**. You are given an integer array `prizePositions` that is **sorted in non-decreasing order**, where `prizePositions[i]` is the position of the <code>i<sup>th</sup></code> prize. There could be different prizes at the same position on the line. You are also given an integer `k`.

You are allowed to select two segments with integer endpoints. The length of each segment must be `k`. You will collect all prizes whose position falls within at least one of the two selected segments (including the endpoints of the segments). The two selected segments may intersect.

*   For example if `k = 2`, you can choose segments `[1, 3]` and `[2, 4]`, and you will win any prize i that satisfies `1 <= prizePositions[i] <= 3` or `2 <= prizePositions[i] <= 4`.

Return _the **maximum** number of prizes you can win if you choose the two segments optimally_.

**Example 1:**

**Input:** prizePositions = [1,1,2,2,3,3,5], k = 2

**Output:** 7

**Explanation:**

In this example, you can win all 7 prizes by selecting two segments [1, 3] and [3, 5].

**Example 2:**

**Input:** prizePositions = [1,2,3,4], k = 0

**Output:** 2

**Explanation:**

For this example, **one choice** for the segments is `[3, 3]` and `[4, 4],` and you will be able to get `2` prizes.

**Constraints:**

*   <code>1 <= prizePositions.length <= 10<sup>5</sup></code>
*   <code>1 <= prizePositions[i] <= 10<sup>9</sup></code>
*   <code>0 <= k <= 10<sup>9</sup></code>
*   `prizePositions` is sorted in non-decreasing order.

## Solution

```java
public class Solution {
    public int maximizeWin(int[] a, int k) {
        int j = 0;
        int i;
        int n = a.length;
        int[] dp = new int[n + 1];
        int ans = 0;
        for (i = 0; i < a.length; i++) {
            while (a[i] - a[j] > k) {
                j++;
            }
            dp[i + 1] = Math.max(dp[i], i - j + 1);
            ans = Math.max(ans, dp[j] + i - j + 1);
        }
        return ans;
    }
}
```