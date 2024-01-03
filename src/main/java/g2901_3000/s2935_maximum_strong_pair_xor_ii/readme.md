[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2935\. Maximum Strong Pair XOR II

Hard

You are given a **0-indexed** integer array `nums`. A pair of integers `x` and `y` is called a **strong** pair if it satisfies the condition:

*   `|x - y| <= min(x, y)`

You need to select two integers from `nums` such that they form a strong pair and their bitwise `XOR` is the **maximum** among all strong pairs in the array.

Return _the **maximum**_ `XOR` _value out of all possible strong pairs in the array_ `nums`.

**Note** that you can pick the same integer twice to form a pair.

**Example 1:**

**Input:** nums = [1,2,3,4,5]

**Output:** 7

**Explanation:** There are 11 strong pairs in the array `nums`: (1, 1), (1, 2), (2, 2), (2, 3), (2, 4), (3, 3), (3, 4), (3, 5), (4, 4), (4, 5) and (5, 5). The maximum XOR possible from these pairs is 3 XOR 4 = 7.

**Example 2:**

**Input:** nums = [10,100]

**Output:** 0

**Explanation:** There are 2 strong pairs in the array nums: (10, 10) and (100, 100). The maximum XOR possible from these pairs is 10 XOR 10 = 0 since the pair (100, 100) also gives 100 XOR 100 = 0.

**Example 3:**

**Input:** nums = [500,520,2500,3000]

**Output:** 1020

**Explanation:** There are 6 strong pairs in the array nums: (500, 500), (500, 520), (520, 520), (2500, 2500), (2500, 3000) and (3000, 3000). The maximum XOR possible from these pairs is 500 XOR 520 = 1020 since the only other non-zero XOR value is 2500 XOR 3000 = 636.

**Constraints:**

*   <code>1 <= nums.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= nums[i] <= 2<sup>20</sup> - 1</code>

## Solution

```java
import java.util.Arrays;
import java.util.BitSet;

public class Solution {
    private final int[] map = new int[1 << 20];

    public int maximumStrongPairXor(int[] nums) {
        Arrays.sort(nums);
        int n = nums.length;
        int max = nums[n - 1];
        int ans = 0;
        int mask;
        int masks = 0;
        int highBit = 20;
        while (--highBit >= 0) {
            if (((max >> highBit) & 1) == 1) {
                break;
            }
        }
        int m = 1 << highBit + 1;
        BitSet seen = new BitSet(m);
        for (int i = highBit; i >= 0; i--) {
            mask = 1 << i;
            masks |= mask;
            if (check(nums, masks, ans | mask, seen)) {
                ans |= mask;
            }
            seen = new BitSet(m);
        }
        return ans;
    }

    private boolean check(int[] nums, int masks, int ans, BitSet seen) {
        for (int x : nums) {
            int mask = x & masks;
            if (seen.get(mask ^ ans) && x <= 2 * map[mask ^ ans]) {
                return true;
            }
            seen.set(mask);
            map[mask] = x;
        }
        return false;
    }
}
```