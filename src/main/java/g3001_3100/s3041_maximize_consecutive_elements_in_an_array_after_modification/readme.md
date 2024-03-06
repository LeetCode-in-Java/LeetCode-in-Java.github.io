[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3041\. Maximize Consecutive Elements in an Array After Modification

Hard

You are given a **0-indexed** array `nums` consisting of **positive** integers.

Initially, you can increase the value of **any** element in the array by **at most** `1`.

After that, you need to select **one or more** elements from the final array such that those elements are **consecutive** when sorted in increasing order. For example, the elements `[3, 4, 5]` are consecutive while `[3, 4, 6]` and `[1, 1, 2, 3]` are not.

Return _the **maximum** number of elements that you can select_.

**Example 1:**

**Input:** nums = [2,1,5,1,1]

**Output:** 3

**Explanation:** We can increase the elements at indices 0 and 3. The resulting array is nums = [3,1,5,2,1].

We select the elements [<ins>**3**</ins>,<ins>**1**</ins>,5,<ins>**2**</ins>,1] and we sort them to obtain [1,2,3], which are consecutive.

It can be shown that we cannot select more than 3 consecutive elements.

**Example 2:**

**Input:** nums = [1,4,7,10]

**Output:** 1

**Explanation:** The maximum consecutive elements that we can select is 1. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>6</sup></code>

## Solution

```java
public class Solution {
    public int maxSelectedElements(int[] nums) {
        int max = 0;
        int min = Integer.MAX_VALUE;
        for (int x : nums) {
            max = Math.max(x, max);
            min = Math.min(x, min);
        }
        int[] count = new int[max + 1];
        for (int x : nums) {
            ++count[x];
        }
        int[] dp = new int[max + 2];
        int ans = 0;
        for (int x = min; x <= max; ++x) {
            if (count[x] == 0) {
                continue;
            }
            int c = count[x];
            if (c == 1) {
                dp[x + 1] = dp[x] + 1;
                dp[x] = dp[x - 1] + 1;
            } else {
                dp[x] = dp[x - 1] + 1;
                dp[x + 1] = dp[x] + 1;
            }
            ans = Math.max(ans, dp[x]);
            ans = Math.max(ans, dp[x + 1]);
        }
        return ans;
    }
}
```