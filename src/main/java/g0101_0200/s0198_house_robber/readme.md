[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 198\. House Robber

Medium

You are a professional robber planning to rob houses along a street. Each house has a certain amount of money stashed, the only constraint stopping you from robbing each of them is that adjacent houses have security systems connected and **it will automatically contact the police if two adjacent houses were broken into on the same night**.

Given an integer array `nums` representing the amount of money of each house, return _the maximum amount of money you can rob tonight **without alerting the police**_.

**Example 1:**

**Input:** nums = [1,2,3,1]

**Output:** 4

**Explanation:**

    Rob house 1 (money = 1) and then rob house 3 (money = 3).
    Total amount you can rob = 1 + 3 = 4. 

**Example 2:**

**Input:** nums = [2,7,9,3,1]

**Output:** 12

**Explanation:**

    Rob house 1 (money = 2), rob house 3 (money = 9) and rob house 5 (money = 1).
    Total amount you can rob = 2 + 9 + 1 = 12. 

**Constraints:**

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 400`

## Solution

```java
public class Solution {
    public int rob(int[] nums) {
        if (nums.length == 0) {
            return 0;
        }
        if (nums.length == 1) {
            return nums[0];
        }
        if (nums.length == 2) {
            return Math.max(nums[0], nums[1]);
        }
        int[] profit = new int[nums.length];
        profit[0] = nums[0];
        profit[1] = Math.max(nums[1], nums[0]);
        for (int i = 2; i < nums.length; i++) {
            profit[i] = Math.max(profit[i - 1], nums[i] + profit[i - 2]);
        }
        return profit[nums.length - 1];
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(n), where n is the length of the input array `nums`. Here's why:

1. The program iterates through the `nums` array once in a loop that runs from 2 to `n - 1`, where n is the length of the array. This loop computes the maximum profit that can be obtained up to each house.

2. In each iteration of the loop, the program calculates `profit[i]` based on the previous two values: `profit[i - 1]` and `profit[i - 2]`. These calculations are done in constant time for each house.

3. Therefore, the total number of operations in the loop is proportional to the length of the `nums` array, resulting in a time complexity of O(n).

**Space Complexity (Big O Space):**

The space complexity of this program is O(n), where n is the length of the input array `nums`. Here's why:

1. The program creates an integer array `profit` of the same length as `nums` to store the maximum profit at each house. Therefore, the space required for the `profit` array is O(n).

2. In addition to the `profit` array, the program uses a constant amount of space for other variables and calculations. These additional space requirements are independent of the size of the input and can be considered O(1).

3. Overall, the dominant factor in space complexity is the `profit` array, resulting in a space complexity of O(n).

In summary, the time complexity of the program is O(n) because it iterates through the input array once, and the space complexity is also O(n) due to the `profit` array used to store intermediate results.
