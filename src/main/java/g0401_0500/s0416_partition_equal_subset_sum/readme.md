## 416\. Partition Equal Subset Sum

Medium

Given a **non-empty** array `nums` containing **only positive integers**, find if the array can be partitioned into two subsets such that the sum of elements in both subsets is equal.

**Example 1:**

**Input:** nums = [1,5,11,5]

**Output:** true

**Explanation:** The array can be partitioned as [1, 5, 5] and [11]. 

**Example 2:**

**Input:** nums = [1,2,3,5]

**Output:** false

**Explanation:** The array cannot be partitioned into equal sum subsets. 

**Constraints:**

*   `1 <= nums.length <= 200`
*   `1 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    public boolean canPartition(int[] nums) {
        int sum = 0;
        for (int num : nums) {
            sum = sum + num;
        }
        if (sum % 2 != 0) {
            return false;
        }
        sum = sum / 2;
        // if use primitive boolean array will make default value to false
        // we need the default value "null" to help us to do the memo
        Boolean[] dp = new Boolean[sum + 1];
        return sumTo(nums, sum, 0, dp);
    }

    private boolean sumTo(int[] nums, int sum, int index, Boolean[] dp) {
        if (sum == 0) {
            return true;
        }
        if (sum < 0) {
            return false;
        }
        if (index == nums.length) {
            return false;
        }
        if (dp[sum] != null) {
            return dp[sum];
        }
        // use the number or not use the number
        dp[sum] = sumTo(nums, sum - nums[index], index + 1, dp) || sumTo(nums, sum, index + 1, dp);
        return dp[sum];
    }
}
```