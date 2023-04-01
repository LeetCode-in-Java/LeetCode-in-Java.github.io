[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2344\. Minimum Deletions to Make Array Divisible

Hard

You are given two positive integer arrays `nums` and `numsDivide`. You can delete any number of elements from `nums`.

Return _the **minimum** number of deletions such that the **smallest** element in_ `nums` _**divides** all the elements of_ `numsDivide`. If this is not possible, return `-1`.

Note that an integer `x` divides `y` if `y % x == 0`.

**Example 1:**

**Input:** nums = [2,3,2,4,3], numsDivide = [9,6,9,3,15]

**Output:** 2

**Explanation:**

The smallest element in [2,3,2,4,3] is 2, which does not divide all the elements of numsDivide.

We use 2 deletions to delete the elements in nums that are equal to 2 which makes nums = [3,4,3].

The smallest element in [3,4,3] is 3, which divides all the elements of numsDivide.

It can be shown that 2 is the minimum number of deletions needed. 

**Example 2:**

**Input:** nums = [4,3,6], numsDivide = [8,2,6,10]

**Output:** -1

**Explanation:**

We want the smallest element in nums to divide all the elements of numsDivide.

There is no way to delete elements from nums to allow this.

**Constraints:**

*   <code>1 <= nums.length, numsDivide.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i], numsDivide[i] <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    public int minOperations(int[] nums, int[] numsDivide) {
        int g = numsDivide[0];
        for (int i : numsDivide) {
            g = gcd(g, i);
        }
        int minOp = 0;
        int smallest = Integer.MAX_VALUE;
        for (int num : nums) {
            if (g % num == 0) {
                smallest = Math.min(smallest, num);
            }
        }
        for (int num : nums) {
            if (num < smallest) {
                ++minOp;
            }
        }
        return smallest == Integer.MAX_VALUE ? -1 : minOp;
    }

    private int gcd(int a, int b) {
        while (b > 0) {
            int tmp = a;
            a = b;
            b = tmp % b;
        }
        return a;
    }
}
```