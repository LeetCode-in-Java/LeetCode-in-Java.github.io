[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3115\. Maximum Prime Difference

Medium

You are given an integer array `nums`.

Return an integer that is the **maximum** distance between the **indices** of two (not necessarily different) prime numbers in `nums`_._

**Example 1:**

**Input:** nums = [4,2,9,5,3]

**Output:** 3

**Explanation:** `nums[1]`, `nums[3]`, and `nums[4]` are prime. So the answer is `|4 - 1| = 3`.

**Example 2:**

**Input:** nums = [4,8,2,8]

**Output:** 0

**Explanation:** `nums[2]` is prime. Because there is just one prime number, the answer is `|2 - 2| = 0`.

**Constraints:**

*   <code>1 <= nums.length <= 3 * 10<sup>5</sup></code>
*   `1 <= nums[i] <= 100`
*   The input is generated such that the number of prime numbers in the `nums` is at least one.

## Solution

```java
public class Solution {
    public int maximumPrimeDifference(int[] nums) {
        int n = nums.length;
        int i = 0;
        while (i < n && check(nums[i])) {
            i++;
        }
        int j = n - 1;
        while (j >= 0 && check(nums[j])) {
            j--;
        }
        return j - i;
    }

    private boolean check(int n) {
        if (n < 2) {
            return true;
        }
        for (int i = 2; i <= Math.sqrt(n); i++) {
            if (n % i == 0) {
                return true;
            }
        }
        return false;
    }
}
```