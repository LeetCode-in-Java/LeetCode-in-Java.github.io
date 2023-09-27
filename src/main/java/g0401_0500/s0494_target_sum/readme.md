[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 494\. Target Sum

Medium

You are given an integer array `nums` and an integer `target`.

You want to build an **expression** out of nums by adding one of the symbols `'+'` and `'-'` before each integer in nums and then concatenate all the integers.

*   For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

Return the number of different **expressions** that you can build, which evaluates to `target`.

**Example 1:**

**Input:** nums = [1,1,1,1,1], target = 3

**Output:** 5

**Explanation:**

    There are 5 ways to assign symbols to make the sum of nums be target 3.
    -1 + 1 + 1 + 1 + 1 = 3
    +1 - 1 + 1 + 1 + 1 = 3
    +1 + 1 - 1 + 1 + 1 = 3
    +1 + 1 + 1 - 1 + 1 = 3
    +1 + 1 + 1 + 1 - 1 = 3 

**Example 2:**

**Input:** nums = [1], target = 1

**Output:** 1 

**Constraints:**

*   `1 <= nums.length <= 20`
*   `0 <= nums[i] <= 1000`
*   `0 <= sum(nums[i]) <= 1000`
*   `-1000 <= target <= 1000`

## Solution

```java
public class Solution {
    public int findTargetSumWays(int[] nums, int s) {
        int sum = 0;
        s = Math.abs(s);
        for (int num : nums) {
            sum += num;
        }
        // Invalid s, just return 0
        if (s > sum || (sum + s) % 2 != 0) {
            return 0;
        }
        int[][] dp = new int[(sum + s) / 2 + 1][nums.length + 1];
        dp[0][0] = 1;
        // empty knapsack must be processed specially
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                dp[0][i + 1] = dp[0][i] * 2;
            } else {
                dp[0][i + 1] = dp[0][i];
            }
        }
        for (int i = 1; i < dp.length; i++) {
            for (int j = 0; j < nums.length; j++) {
                dp[i][j + 1] += dp[i][j];
                if (nums[j] <= i) {
                    dp[i][j + 1] += dp[i - nums[j]][j];
                }
            }
        }
        return dp[(sum + s) / 2][nums.length];
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of the program is determined by the following major components:

1. Calculating the total sum of elements in the 'nums' array: This requires iterating through all the elements of 'nums', so it has a time complexity of O(n), where 'n' is the length of the 'nums' array.

2. Checking for invalid 's': The conditionals that check if 's' is invalid and returning 0 have constant time complexity, O(1).

3. Initializing the dynamic programming (DP) table 'dp': The initialization of the 'dp' table involves iterating through the 'nums' array once. This step has a time complexity of O(n), where 'n' is the length of the 'nums' array.

4. Filling in the DP table: The nested loops that fill in the 'dp' table have a time complexity of O(n * (sum + s)), where 'n' is the length of the 'nums' array, 'sum' is the sum of elements in 'nums', and 's' is the target sum.

Overall, the dominant factor in the time complexity is the nested loops that fill in the 'dp' table, and their time complexity is O(n * (sum + s)). In the worst case, 'sum' and 's' could both be large, leading to a time complexity that grows with the product of 'n', 'sum', and 's'.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the following major components:

1. Integer variables 'sum' and 's': These variables require constant space, O(1).

2. The DP table 'dp': The 'dp' table is a two-dimensional array of size [(sum + s) / 2 + 1][n + 1], where 'n' is the length of the 'nums' array. Therefore, the space complexity of the 'dp' table is O(n * (sum + s)).

3. Integer variables used in loops: These variables require constant space, O(1).

Overall, the dominant factor in the space complexity is the 'dp' table, and its space complexity is O(n * (sum + s)).

In summary, the provided program has a time complexity of O(n * (sum + s)) and a space complexity of O(n * (sum + s)), where 'n' is the length of the 'nums' array, 'sum' is the sum of elements in 'nums', and 's' is the target sum. The time complexity grows with the product of 'n', 'sum', and 's', making it potentially high for large inputs.
