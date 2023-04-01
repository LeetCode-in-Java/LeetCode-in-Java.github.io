[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 368\. Largest Divisible Subset

Medium

Given a set of **distinct** positive integers `nums`, return the largest subset `answer` such that every pair `(answer[i], answer[j])` of elements in this subset satisfies:

*   `answer[i] % answer[j] == 0`, or
*   `answer[j] % answer[i] == 0`

If there are multiple solutions, return any of them.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** [1,2]

**Explanation:** [1,3] is also accepted.

**Example 2:**

**Input:** nums = [1,2,4,8]

**Output:** [1,2,4,8]

**Constraints:**

*   `1 <= nums.length <= 1000`
*   <code>1 <= nums[i] <= 2 * 10<sup>9</sup></code>
*   All the integers in `nums` are **unique**.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Solution {
    // Helper class containing value and an arraylist
    private static class Helper {
        int val;
        List<Integer> al;

        Helper(int val) {
            this.val = val;
            al = new ArrayList<>();
        }
    }

    public List<Integer> largestDivisibleSubset(int[] nums) {
        // Initializing Helper type array
        Helper[] dp = new Helper[nums.length];
        // Sorting given array
        Arrays.sort(nums);
        // max variable will keep track size of Longest Divisible Subset
        int max = 0;
        // index variable will keep track the index at which dp contains Longest Divisible Subset
        int index = 0;
        dp[0] = new Helper(1);
        dp[0].al.add(nums[0]);
        // Operation similar to LIS
        for (int i = 1; i < nums.length; i++) {
            int max2 = 0;
            int pos = i;
            for (int j = 0; j < i; j++) {
                if (nums[i] % nums[j] == 0 && max2 < dp[j].val) {
                    max2 = dp[j].val;
                    pos = j;
                }
            }
            // max2 = 0, means no element exists which can divide the present element
            if (max2 == 0) {
                dp[i] = new Helper(max2 + 1);
                dp[i].al.add(nums[i]);
            } else {
                dp[i] = new Helper(max2 + 1);
                for (int val : dp[pos].al) {
                    dp[i].al.add(val);
                }
                dp[i].al.add(nums[i]);
            }
            if (max2 > max) {
                max = max2;
                index = i;
            }
        }
        // Returning Largest Divisible Subset
        return dp[index].al;
    }
}
```