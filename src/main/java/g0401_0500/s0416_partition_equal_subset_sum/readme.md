[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

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
        int sums = 0;
        for (int num : nums) {
            sums += num;
        }
        if (sums % 2 == 1) {
            return false;
        }
        sums /= 2;
        boolean[] dp = new boolean[sums + 1];
        dp[0] = true;
        for (int num : nums) {
            for (int sum = sums; sum >= num; sum--) {
                dp[sum] = dp[sum] || dp[sum - num];
            }
        }
        return dp[sums];
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of the program depends on two main factors:

1. The sum of all elements in the input array: The program iterates through the array to calculate the sum of all elements. This step takes O(n) time, where 'n' is the number of elements in the array.

2. Dynamic Programming (DP) Loop: The program uses a dynamic programming approach to determine if a subset sum (less than or equal to half of the total sum) can be achieved. It uses a 2D boolean array 'dp' with dimensions (n+1) x (sums/2+1), where 'n' is the number of elements in the array, and 'sums' is the sum of all elements. The nested loops that iterate through 'num' and 'sum' take O(n * sums) time.

Combining these two factors, the overall time complexity of the program is O(n * sums), where 'n' is the number of elements in the input array, and 'sums' is the sum of all elements.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the additional data structures used:

1. Integer variable 'sums': This variable stores the sum of all elements in the input array. It requires O(1) space.

2. Boolean array 'dp': This 2D array has dimensions (n+1) x (sums/2+1). In the worst case, the size of 'dp' is proportional to 'n' and the sum of all elements in the input array. Therefore, the space complexity due to 'dp' is O(n * sums).

3. Various integer variables used for iteration and intermediate calculations: These variables require constant space, O(1).

Combining these factors, the dominant factor in the space complexity is the 'dp' array, which can grow to O(n * sums) in size in the worst case. Therefore, the space complexity of the program is O(n * sums).

In summary, the provided program has a time complexity of O(n * sums) and a space complexity of O(n * sums), where 'n' is the number of elements in the input array, and 'sums' is the sum of all elements.
