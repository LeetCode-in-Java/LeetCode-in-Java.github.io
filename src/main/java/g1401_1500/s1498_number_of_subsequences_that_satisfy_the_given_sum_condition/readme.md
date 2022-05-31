## 1498\. Number of Subsequences That Satisfy the Given Sum Condition

Medium

You are given an array of integers `nums` and an integer `target`.

Return _the number of **non-empty** subsequences of_ `nums` _such that the sum of the minimum and maximum element on it is less or equal to_ `target`. Since the answer may be too large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** nums = [3,5,6,7], target = 9

**Output:** 4

**Explanation:** There are 4 subsequences that satisfy the condition.

[3] -> Min value + max value <= target (3 + 3 <= 9)

[3,5] -> (3 + 5 <= 9)

[3,5,6] -> (3 + 6 <= 9)

[3,6] -> (3 + 6 <= 9)

**Example 2:**

**Input:** nums = [3,3,6,8], target = 10

**Output:** 6

**Explanation:** There are 6 subsequences that satisfy the condition. (nums can have repeated numbers). [3] , [3] , [3,3], [3,6] , [3,6] , [3,3,6]

**Example 3:**

**Input:** nums = [2,3,3,4,6,7], target = 12

**Output:** 61

**Explanation:** There are 63 non-empty subsequences, two of them do not satisfy the condition ([6,7], [7]). Number of valid subsequences (63 - 2 = 61).

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>
*   <code>1 <= target <= 10<sup>6</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int numSubseq(int[] nums, int target) {
        // sorted array will be used to perform binary search
        Arrays.sort(nums);
        int mod = 1000_000_007;
        // powOf2[i] means (2^i) % mod
        int[] powOf2 = new int[nums.length];
        powOf2[0] = 1;
        for (int i = 1; i < nums.length; i++) {
            powOf2[i] = (powOf2[i - 1] * 2) % mod;
        }
        int res = 0;
        int left = 0;
        int right = nums.length - 1;
        while (left <= right) {
            if (nums[left] + nums[right] > target) {
                // nums[right] which is macimum is too big so decrease it
                right--;
            } else {
                // every number between right and left be either picked or not picked
                // so that is why pow(2, right - left) essentially
                res = (res + powOf2[right - left]) % mod;
                // increment left to find next set of min and max
                left++;
            }
        }
        return res;
    }
}
```