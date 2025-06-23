[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3591\. Check if Any Element Has Prime Frequency

Easy

You are given an integer array `nums`.

Return `true` if the frequency of any element of the array is **prime**, otherwise, return `false`.

The **frequency** of an element `x` is the number of times it occurs in the array.

A prime number is a natural number greater than 1 with only two factors, 1 and itself.

**Example 1:**

**Input:** nums = [1,2,3,4,5,4]

**Output:** true

**Explanation:**

4 has a frequency of two, which is a prime number.

**Example 2:**

**Input:** nums = [1,2,3,4,5]

**Output:** false

**Explanation:**

All elements have a frequency of one.

**Example 3:**

**Input:** nums = [2,2,2,4,4]

**Output:** true

**Explanation:**

Both 2 and 4 have a prime frequency.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `0 <= nums[i] <= 100`

## Solution

```java
public class Solution {
    private boolean isPrime(int n) {
        if (n <= 1) {
            return false;
        }
        if (n == 2 || n == 3) {
            return true;
        }
        for (int i = 2; i < n; i++) {
            if (n % i == 0) {
                return false;
            }
        }
        return true;
    }

    public boolean checkPrimeFrequency(int[] nums) {
        int n = nums.length;
        if (n == 1) {
            return false;
        }
        int maxi = Integer.MIN_VALUE;
        for (int val : nums) {
            maxi = Math.max(val, maxi);
        }
        int[] hash = new int[maxi + 1];
        for (int num : nums) {
            hash[num]++;
        }
        for (int j : hash) {
            if (isPrime(j)) {
                return true;
            }
        }
        return false;
    }
}
```