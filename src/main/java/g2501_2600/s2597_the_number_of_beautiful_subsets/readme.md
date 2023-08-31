[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2597\. The Number of Beautiful Subsets

Medium

You are given an array `nums` of positive integers and a **positive** integer `k`.

A subset of `nums` is **beautiful** if it does not contain two integers with an absolute difference equal to `k`.

Return _the number of **non-empty beautiful** subsets of the array_ `nums`.

A **subset** of `nums` is an array that can be obtained by deleting some (possibly none) elements from `nums`. Two subsets are different if and only if the chosen indices to delete are different.

**Example 1:**

**Input:** nums = [2,4,6], k = 2

**Output:** 4

**Explanation:**

The beautiful subsets of the array nums are: [2], [4], [6], [2, 6].

It can be proved that there are only 4 beautiful subsets in the array [2,4,6].

**Example 2:**

**Input:** nums = [1], k = 1

**Output:** 1

**Explanation:**

The beautiful subset of the array nums is [1].

It can be proved that there is only 1 beautiful subset in the array [1].

**Constraints:**

*   `1 <= nums.length <= 20`
*   `1 <= nums[i], k <= 1000`

## Solution

```java
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Solution {
    public int beautifulSubsets(int[] nums, int k) {
        Map<Integer, Integer> map = new HashMap<>();
        for (int n : nums) {
            map.put(n, map.getOrDefault(n, 0) + 1);
        }
        int res = 1;
        for (Map.Entry<Integer, Integer> entry : map.entrySet()) {
            if (!map.containsKey(entry.getKey() - k)) {
                if (map.containsKey(entry.getKey() + k)) {
                    List<Integer> freq = new ArrayList<>();
                    int localKey = entry.getKey();
                    while (map.containsKey(localKey)) {
                        freq.add(map.get(localKey));
                        localKey += k;
                    }
                    res *= helper(freq);
                } else {
                    res *= 1 << entry.getValue();
                }
            }
        }
        return res - 1;
    }

    private int helper(List<Integer> freq) {
        int n = freq.size();
        if (n == 1) {
            return 1 << freq.get(0);
        }
        int[] dp = new int[n];
        dp[0] = (1 << freq.get(0)) - 1;
        dp[1] = (1 << freq.get(1)) - 1;
        if (n == 2) {
            return dp[0] + dp[1] + 1;
        }
        for (int i = 2; i < n; i++) {
            if (i > 2) {
                dp[i - 2] += dp[i - 3];
            }

            dp[i] = (dp[i - 2] + 1) * ((1 << freq.get(i)) - 1);
        }
        return dp[n - 1] + dp[n - 2] + dp[n - 3] + 1;
    }
}
```