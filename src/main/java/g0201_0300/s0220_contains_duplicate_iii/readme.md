[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 220\. Contains Duplicate III

Medium

Given an integer array `nums` and two integers `k` and `t`, return `true` if there are **two distinct indices** `i` and `j` in the array such that `abs(nums[i] - nums[j]) <= t` and `abs(i - j) <= k`.

**Example 1:**

**Input:** nums = [1,2,3,1], k = 3, t = 0

**Output:** true 

**Example 2:**

**Input:** nums = [1,0,1,1], k = 1, t = 2

**Output:** true 

**Example 3:**

**Input:** nums = [1,5,9,1,5,9], k = 2, t = 3

**Output:** false 

**Constraints:**

*   <code>1 <= nums.length <= 2 * 10<sup>4</sup></code>
*   <code>-2<sup>31</sup> <= nums[i] <= 2<sup>31</sup> - 1</code>
*   <code>0 <= k <= 10<sup>4</sup></code>
*   <code>0 <= t <= 2<sup>31</sup> - 1</code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private long getId(long i, long w) {
        return i < 0 ? (i + 1) / w - 1 : i / w;
    }

    public boolean containsNearbyAlmostDuplicate(int[] nums, int k, int t) {
        if (t < 0) {
            return false;
        }
        Map<Long, Long> d = new HashMap<>();
        long w = (long) t + 1;
        for (int i = 0; i < nums.length; ++i) {
            long m = getId(nums[i], w);
            if (d.containsKey(m)) {
                return true;
            }
            if (d.containsKey(m - 1) && Math.abs(nums[i] - d.get(m - 1)) < w) {
                return true;
            }
            if (d.containsKey(m + 1) && Math.abs(nums[i] - d.get(m + 1)) < w) {
                return true;
            }
            d.put(m, (long) nums[i]);
            if (i >= k) {
                d.remove(getId(nums[i - k], w));
            }
        }
        return false;
    }
}
```