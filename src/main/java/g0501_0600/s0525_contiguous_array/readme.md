[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 525\. Contiguous Array

Medium

Given a binary array `nums`, return _the maximum length of a contiguous subarray with an equal number of_ `0` _and_ `1`.

**Example 1:**

**Input:** nums = [0,1]

**Output:** 2

**Explanation:** [0, 1] is the longest contiguous subarray with an equal number of 0 and 1.

**Example 2:**

**Input:** nums = [0,1,0]

**Output:** 2

**Explanation:** [0, 1] (or [1, 0]) is a longest contiguous subarray with equal number of 0 and 1.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   `nums[i]` is either `0` or `1`.

## Solution

```java
import java.util.HashMap;

public class Solution {
    public int findMaxLength(int[] nums) {
        for (int i = 0; i < nums.length; i++) {
            if (nums[i] == 0) {
                nums[i] = -1;
            }
        }
        HashMap<Integer, Integer> map = new HashMap<>();
        map.put(0, -1);
        int ps = 0;
        int len = 0;
        for (int i = 0; i < nums.length; i++) {
            ps += nums[i];
            if (!map.containsKey(ps)) {
                map.put(ps, i);
            } else {
                len = Math.max(len, i - map.get(ps));
            }
        }
        return len;
    }
}
```