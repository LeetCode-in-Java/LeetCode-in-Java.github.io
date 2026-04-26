[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3755\. Find Maximum Balanced XOR Subarray Length

Medium

Given an integer array `nums`, return the **length** of the **longest non-empty subarrays** that has a bitwise XOR of zero and contains an **equal** number of **even** and **odd** numbers. If no such subarray exists, return 0.

**Example 1:**

**Input:** nums = [3,1,3,2,0]

**Output:** 4

**Explanation:**

The subarray `[1, 3, 2, 0]` has bitwise XOR `1 XOR 3 XOR 2 XOR 0 = 0` and contains 2 even and 2 odd numbers.

**Example 2:**

**Input:** nums = [3,2,8,5,4,14,9,15]

**Output:** 8

**Explanation:**

The whole array has bitwise XOR `0` and contains 4 even and 4 odd numbers.

**Example 3:**

**Input:** nums = [0]

**Output:** 0

**Explanation:**

No non-empty subarray satisfies both conditions.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int maxBalancedSubarray(int[] nums) {
        int n = nums.length;
        int ans = 0;
        int xor = 0;
        int diff = n;
        Map<Long, Integer> pos = new HashMap<>(n + 1, 1);
        pos.put((long) xor << 20 | diff, -1);
        for (int i = 0; i < n; i++) {
            xor ^= nums[i];
            diff += nums[i] % 2 != 0 ? 1 : -1;
            long key = (long) xor << 20 | diff;
            Integer j = pos.get(key);
            if (j != null) {
                ans = Math.max(ans, i - j);
            } else {
                pos.put(key, i);
            }
        }
        return ans;
    }
}
```