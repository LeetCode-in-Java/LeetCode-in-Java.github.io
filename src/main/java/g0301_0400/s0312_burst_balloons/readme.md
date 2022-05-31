## 312\. Burst Balloons

Hard

You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the <code>i<sup>th</sup></code> balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return _the maximum coins you can collect by bursting the balloons wisely_.

**Example 1:**

**Input:** nums = [3,1,5,8]

**Output:** 167

**Explanation:**

    nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []
    coins = 3*1*5 + 3*5*8 + 1*3*8 + 1*8*1 = 167

**Example 2:**

**Input:** nums = [1,5]

**Output:** 10 

**Constraints:**

*   `n == nums.length`
*   `1 <= n <= 500`
*   `0 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    public int maxCoins(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        int[][] dp = new int[nums.length][nums.length];
        return balloonBurstDp(nums, dp);
    }

    private int balloonBurstDp(int[] nums, int[][] dp) {
        for (int gap = 0; gap < nums.length; gap++) {
            for (int si = 0, ei = gap; ei < nums.length; si++, ei++) {
                int l = (si - 1 == -1) ? 1 : nums[si - 1];
                int r = (ei + 1 == nums.length) ? 1 : nums[ei + 1];
                int maxAns = (int) -1e7;
                for (int cut = si; cut <= ei; cut++) {
                    int leftAns = si == cut ? 0 : dp[si][cut - 1];
                    int rightAns = ei == cut ? 0 : dp[cut + 1][ei];
                    int myAns = leftAns + l * nums[cut] * r + rightAns;
                    maxAns = Math.max(maxAns, myAns);
                }
                dp[si][ei] = maxAns;
            }
        }
        return dp[0][nums.length - 1];
    }
}
```