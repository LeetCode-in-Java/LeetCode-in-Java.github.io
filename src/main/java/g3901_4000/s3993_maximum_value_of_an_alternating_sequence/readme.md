[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3993\. Maximum Value of an Alternating Sequence

Medium

You are given three integers `n`, `s`, and `m`.

A sequence `seq` of integers of length `n` is considered **valid** if:

*   `seq[0] = s`.
*   The sequence is **alternating**, meaning that either:
    *   `seq[0] > seq[1] < seq[2] > ...`, or
    *   `seq[0] < seq[1] > seq[2] < ...`.
*   For every adjacent pair, `|seq[i] - seq[i - 1]| <= m`.

A sequence of length 1 is considered alternating.

Return the **maximum** possible element that can appear in any valid sequence.

OriginalJava**Example 1:**

**Input:** n = 4, s = 3, m = 5

**Output:** 12

**Explanation:**

*   One valid sequence is `[3, 8, 7, 12]`.
*   The maximum element in the sequence is 12.

**Example 2:**

**Input:** n = 2, s = 4, m = 3

**Output:** 7

**Explanation:**

*   One valid sequence is `[4, 7]`.
*   The maximum element in the sequence is 7.

**Constraints:**

*   <code>1 <= n, s <= 10<sup>9</sup></code>
*   <code>1 <= m <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public long maximumValue(int n, int s, int m) {
        if (n == 1) {
            return s;
        }
        return s + (long) (n / 2) * (m - 1) + 1;
    }
}
```