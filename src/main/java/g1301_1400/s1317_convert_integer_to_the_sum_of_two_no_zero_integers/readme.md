[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1317\. Convert Integer to the Sum of Two No-Zero Integers

Easy

**No-Zero integer** is a positive integer that **does not contain any `0`** in its decimal representation.

Given an integer `n`, return _a list of two integers_ `[A, B]` _where_:

*   `A` and `B` are **No-Zero integers**.
*   `A + B = n`

The test cases are generated so that there is at least one valid solution. If there are many valid solutions you can return any of them.

**Example 1:**

**Input:** n = 2

**Output:** [1,1]

**Explanation:** A = 1, B = 1. A + B = n and both A and B do not contain any 0 in their decimal representation.

**Example 2:**

**Input:** n = 11

**Output:** [2,9]

**Constraints:**

*   <code>2 <= n <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int[] getNoZeroIntegers(int n) {
        int left = 1;
        int right = n - 1;
        while (left <= right) {
            if (noZero(left) && noZero(right)) {
                return new int[] {left, right};
            } else {
                left++;
                right--;
            }
        }
        return new int[] {};
    }

    private boolean noZero(int num) {
        while (num != 0) {
            if (num % 10 == 0) {
                return false;
            } else {
                num /= 10;
            }
        }
        return true;
    }
}
```