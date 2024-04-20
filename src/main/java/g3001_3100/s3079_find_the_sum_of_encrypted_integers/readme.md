[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3079\. Find the Sum of Encrypted Integers

Easy

You are given an integer array `nums` containing **positive** integers. We define a function `encrypt` such that `encrypt(x)` replaces **every** digit in `x` with the **largest** digit in `x`. For example, `encrypt(523) = 555` and `encrypt(213) = 333`.

Return _the **sum** of encrypted elements_.

**Example 1:**

**Input:** nums = [1,2,3]

**Output:** 6

**Explanation:** The encrypted elements are `[1,2,3]`. The sum of encrypted elements is `1 + 2 + 3 == 6`.

**Example 2:**

**Input:** nums = [10,21,31]

**Output:** 66

**Explanation:** The encrypted elements are `[11,22,33]`. The sum of encrypted elements is `11 + 22 + 33 == 66`.

**Constraints:**

*   `1 <= nums.length <= 50`
*   `1 <= nums[i] <= 1000`

## Solution

```java
public class Solution {
    private int encrypt(int x) {
        int nDigits = 0;
        int max = 0;
        while (x > 0) {
            max = Math.max(max, x % 10);
            x /= 10;
            nDigits++;
        }
        int ans = 0;
        for (int i = 0; i < nDigits; i++) {
            ans = ans * 10 + max;
        }
        return ans;
    }

    public int sumOfEncryptedInt(int[] nums) {
        int ret = 0;
        for (int num : nums) {
            ret += encrypt(num);
        }
        return ret;
    }
}
```