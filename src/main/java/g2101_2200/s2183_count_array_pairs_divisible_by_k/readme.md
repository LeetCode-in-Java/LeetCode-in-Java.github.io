[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2183\. Count Array Pairs Divisible by K

Hard

Given a **0-indexed** integer array `nums` of length `n` and an integer `k`, return _the **number of pairs**_ `(i, j)` _such that:_

*   `0 <= i < j <= n - 1` _and_
*   `nums[i] * nums[j]` _is divisible by_ `k`.

**Example 1:**

**Input:** nums = [1,2,3,4,5], k = 2

**Output:** 7

**Explanation:**

The 7 pairs of indices whose corresponding products are divisible by 2 are

(0, 1), (0, 3), (1, 2), (1, 3), (1, 4), (2, 3), and (3, 4).

Their products are 2, 4, 6, 8, 10, 12, and 20 respectively.

Other pairs such as (0, 2) and (2, 4) have products 3 and 15 respectively, which are not divisible by 2. 

**Example 2:**

**Input:** nums = [1,2,3,4], k = 5

**Output:** 0

**Explanation:** There does not exist any pair of indices whose corresponding product is divisible by 5. 

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], k <= 10<sup>5</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

@SuppressWarnings("java:S2234")
public class Solution {
    public long countPairs(int[] nums, int k) {
        long count = 0L;
        Map<Integer, Long> map = new HashMap<>();
        for (int num : nums) {
            int gd = gcd(num, k);
            int want = k / gd;
            for (Map.Entry<Integer, Long> entry : map.entrySet()) {
                if (entry.getKey() % want == 0) {
                    count += entry.getValue();
                }
            }
            map.put(gd, map.getOrDefault(gd, 0L) + 1L);
        }
        return count;
    }

    private int gcd(int a, int b) {
        if (a > b) {
            return gcd(b, a);
        }
        if (a == 0) {
            return b;
        }
        return gcd(a, b % a);
    }
}
```