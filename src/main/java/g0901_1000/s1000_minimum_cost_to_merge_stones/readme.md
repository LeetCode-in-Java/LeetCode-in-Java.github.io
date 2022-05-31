## 1000\. Minimum Cost to Merge Stones

Hard

There are `n` piles of `stones` arranged in a row. The <code>i<sup>th</sup></code> pile has `stones[i]` stones.

A move consists of merging exactly `k` consecutive piles into one pile, and the cost of this move is equal to the total number of stones in these `k` piles.

Return _the minimum cost to merge all piles of stones into one pile_. If it is impossible, return `-1`.

**Example 1:**

**Input:** stones = [3,2,4,1], k = 2

**Output:** 20

**Explanation:** We start with [3, 2, 4, 1].

We merge [3, 2] for a cost of 5, and we are left with [5, 4, 1].

We merge [4, 1] for a cost of 5, and we are left with [5, 5].

We merge [5, 5] for a cost of 10, and we are left with [10].

The total cost was 20, and this is the minimum possible.

**Example 2:**

**Input:** stones = [3,2,4,1], k = 3

**Output:** -1

**Explanation:** After any merge operation, there are 2 piles left, and we can't merge anymore. So the task is impossible.

**Example 3:**

**Input:** stones = [3,5,1,2,6], k = 3

**Output:** 25

**Explanation:** We start with [3, 5, 1, 2, 6].

We merge [5, 1, 2] for a cost of 8, and we are left with [3, 8, 6].

We merge [3, 8, 6] for a cost of 17, and we are left with [17].

The total cost was 25, and this is the minimum possible.

**Constraints:**

*   `n == stones.length`
*   `1 <= n <= 30`
*   `1 <= stones[i] <= 100`
*   `2 <= k <= 30`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int[][] memo;
    private int[] prefixSum;

    public int mergeStones(int[] stones, int k) {
        int n = stones.length;
        if ((n - 1) % (k - 1) != 0) {
            return -1;
        }
        memo = new int[n][n];
        for (int[] arr : memo) {
            Arrays.fill(arr, -1);
        }
        prefixSum = new int[n + 1];
        for (int i = 1; i < n + 1; i++) {
            prefixSum[i] = prefixSum[i - 1] + stones[i - 1];
        }
        return dp(0, n - 1, k);
    }

    private int dp(int left, int right, int k) {
        if (memo[left][right] > 0) {
            return memo[left][right];
        }
        if (right - left + 1 < k) {
            memo[left][right] = 0;
            return memo[left][right];
        }
        if (right - left + 1 == k) {
            memo[left][right] = prefixSum[right + 1] - prefixSum[left];
            return memo[left][right];
        }
        int val = Integer.MAX_VALUE;
        for (int i = 0; left + i + 1 <= right; i += k - 1) {
            val = Math.min(val, dp(left, left + i, k) + dp(left + i + 1, right, k));
        }
        if ((right - left) % (k - 1) == 0) {
            val += prefixSum[right + 1] - prefixSum[left];
        }
        memo[left][right] = val;
        return val;
    }
}
```