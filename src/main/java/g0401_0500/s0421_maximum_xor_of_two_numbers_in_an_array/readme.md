[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 421\. Maximum XOR of Two Numbers in an Array

Medium

Given an integer array `nums`, return _the maximum result of_ `nums[i] XOR nums[j]`, where `0 <= i <= j < n`.

**Example 1:**

**Input:** nums = [3,10,5,25,2,8]

**Output:** 28

**Explanation:** The maximum result is 5 XOR 25 = 28. 

**Example 2:**

**Input:** nums = [14,70,53,83,49,91,36,80,92,51,66,70]

**Output:** 127 

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>5</sup></code>
*   <code>0 <= nums[i] <= 2<sup>31</sup> - 1</code>

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public int findMaximumXOR(int[] nums) {
        int max = 0;
        int mask = 0;
        Set<Integer> set = new HashSet<>();
        int maxNum = 0;
        for (int i : nums) {
            maxNum = Math.max(maxNum, i);
        }
        for (int i = 31 - Integer.numberOfLeadingZeros(maxNum); i >= 0; i--) {
            set.clear();
            int bit = 1 << i;
            mask = mask | bit;
            int temp = max | bit;
            for (int prefix : nums) {
                if (set.contains((prefix & mask) ^ temp)) {
                    max = temp;
                    break;
                }
                set.add(prefix & mask);
            }
        }
        return max;
    }
}
```