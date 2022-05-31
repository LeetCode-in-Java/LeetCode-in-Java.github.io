## 1879\. Minimum XOR Sum of Two Arrays

Hard

You are given two integer arrays `nums1` and `nums2` of length `n`.

The **XOR sum** of the two integer arrays is `(nums1[0] XOR nums2[0]) + (nums1[1] XOR nums2[1]) + ... + (nums1[n - 1] XOR nums2[n - 1])` (**0-indexed**).

*   For example, the **XOR sum** of `[1,2,3]` and `[3,2,1]` is equal to `(1 XOR 3) + (2 XOR 2) + (3 XOR 1) = 2 + 0 + 2 = 4`.

Rearrange the elements of `nums2` such that the resulting **XOR sum** is **minimized**.

Return _the **XOR sum** after the rearrangement_.

**Example 1:**

**Input:** nums1 = [1,2], nums2 = [2,3]

**Output:** 2

**Explanation:** Rearrange `nums2` so that it becomes `[3,2]`. The XOR sum is (1 XOR 3) + (2 XOR 2) = 2 + 0 = 2.

**Example 2:**

**Input:** nums1 = [1,0,3], nums2 = [5,3,4]

**Output:** 8

**Explanation:** Rearrange `nums2` so that it becomes `[5,4,3]`. The XOR sum is (1 XOR 5) + (0 XOR 4) + (3 XOR 3) = 4 + 4 + 0 = 8.

**Constraints:**

*   `n == nums1.length`
*   `n == nums2.length`
*   `1 <= n <= 14`
*   <code>0 <= nums1[i], nums2[i] <= 10<sup>7</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minimumXORSum(int[] nums1, int[] nums2) {
        int l = nums1.length;
        int[] dp = new int[1 << l];
        Arrays.fill(dp, -1);
        dp[0] = 0;
        return dfs(dp.length - 1, l, nums1, nums2, dp, l);
    }

    private int dfs(int state, int length, int[] nums1, int[] nums2, int[] dp, int totalLength) {
        if (dp[state] >= 0) {
            return dp[state];
        }
        int min = Integer.MAX_VALUE;
        int currIndex = totalLength - length;
        int i = 0;
        for (int index = 0; i < length; index++) {
            if (((state >> index) & 1) == 1) {
                int result = dfs(state ^ (1 << index), length - 1, nums1, nums2, dp, totalLength);
                min = Math.min(min, (nums2[currIndex] ^ nums1[index]) + result);
                i++;
            }
        }
        dp[state] = min;
        return min;
    }
}
```