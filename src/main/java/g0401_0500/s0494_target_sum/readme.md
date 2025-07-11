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
    public int findTargetSumWays(int[] nums, int target) {
        int totalSum = 0;
        for (int num : nums) {
            totalSum += num;
        }
        int sum = totalSum - target;
        if (sum < 0 || sum % 2 == 1) {
            return 0;
        }
        return solve(nums, sum / 2);
    }

    private int solve(int[] nums, int target) {
        int[] prev = new int[target + 1];
        if (nums[0] == 0) {
            prev[0] = 2;
        } else {
            prev[0] = 1;
        }
        if (nums[0] != 0 && nums[0] <= target) {
            prev[nums[0]] = 1;
        }
        int n = nums.length;
        for (int i = 1; i < n; i++) {
            int[] curr = new int[target + 1];
            for (int j = 0; j <= target; j++) {
                int taken = 0;
                if (j >= nums[i]) {
                    taken = prev[j - nums[i]];
                }
                int nonTaken = prev[j];
                curr[j] = taken + nonTaken;
            }
            prev = curr;
        }
        return prev[target];
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
