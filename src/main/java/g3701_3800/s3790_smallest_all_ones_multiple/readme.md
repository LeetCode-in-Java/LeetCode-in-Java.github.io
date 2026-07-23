[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3790\. Smallest All-Ones Multiple

Medium

You are given a positive integer `k`.

Find the **smallest** integer `n` divisible by `k` that consists of **only the digit 1** in its decimal representation (e.g., 1, 11, 111, ...).

Return an integer denoting the **number of digits** in the decimal representation of `n`. If no such `n` exists, return `-1`.

**Example 1:**

**Input:** k = 3

**Output:** 3

**Explanation:**

`n = 111` because 111 is divisible by 3, but 1 and 11 are not. The length of `n = 111` is 3.

**Example 2:**

**Input:** k = 7

**Output:** 6

**Explanation:**

`n = 111111`. The length of `n = 111111` is 6.

**Example 3:**

**Input:** k = 2

**Output:** \-1

**Explanation:**

There does not exist a valid `n` that is a multiple of 2.

**Constraints:**

*   <code>2 <= k <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int minAllOneMultiple(int k) {
        if (k % 2 == 0 || k % 5 == 0) {
            return -1;
        }
        int rem = 0;
        for (int i = 1; i <= k; i++) {
            rem = (rem * 10 + 1) % k;
            if (rem == 0) {
                return i;
            }
        }
        return -1;
    }
}
```