[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2601\. Prime Subtraction Operation

Medium

You are given a **0-indexed** integer array `nums` of length `n`.

You can perform the following operation as many times as you want:

*   Pick an index `i` that you haven’t picked before, and pick a prime `p` **strictly less than** `nums[i]`, then subtract `p` from `nums[i]`.

Return _true if you can make `nums` a strictly increasing array using the above operation and false otherwise._

A **strictly increasing array** is an array whose each element is strictly greater than its preceding element.

**Example 1:**

**Input:** nums = [4,9,6,10]

**Output:** true

**Explanation:** In the first operation: Pick i = 0 and p = 3, and then subtract 3 from nums[0], so that nums becomes [1,9,6,10]. In the second operation: i = 1, p = 7, subtract 7 from nums[1], so nums becomes equal to [1,2,6,10]. After the second operation, nums is sorted in strictly increasing order, so the answer is true.

**Example 2:**

**Input:** nums = [6,8,11,12]

**Output:** true

**Explanation:** Initially nums is sorted in strictly increasing order, so we don't need to make any operations.

**Example 3:**

**Input:** nums = [5,8,3]

**Output:** false

**Explanation:** It can be proven that there is no way to perform operations to make nums sorted in strictly increasing order, so the answer is false.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `1 <= nums[i] <= 1000`
*   `nums.length == n`

## Solution

```java
import java.util.Arrays;

public class Solution {
    private int[] primesUntil(int n) {
        if (n < 2) {
            return new int[0];
        }
        int[] primes = new int[200];
        boolean[] composite = new boolean[n + 1];
        primes[0] = 2;
        int added = 1;
        int i = 3;
        while (i <= n) {
            if (composite[i]) {
                i += 2;
                continue;
            }
            primes[added++] = i;
            int j = i * i;
            while (j <= n) {
                composite[j] = true;
                j += i;
            }
            i += 2;
        }
        return Arrays.copyOf(primes, added);
    }

    public boolean primeSubOperation(int[] nums) {
        int max = 0;
        for (int n : nums) {
            max = Math.max(max, n);
        }
        int[] primes = primesUntil(max);
        int prev = 0;
        for (int n : nums) {
            int pos = Arrays.binarySearch(primes, n - prev - 1);
            if (pos == -1 && n <= prev) {
                return false;
            }
            final int index;
            if (pos == -1) {
                index = 0;
            } else {
                index = pos < 0 ? primes[-pos - 2] : primes[pos];
            }
            prev = n - index;
        }
        return true;
    }
}
```