[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3726\. Remove Zeros in Decimal Representation

Easy

You are given a **positive** integer `n`.

Return the integer obtained by removing all zeros from the decimal representation of `n`.

**Example 1:**

**Input:** n = 1020030

**Output:** 123

**Explanation:**

After removing all zeros from 1**<ins>0</ins>**2**<ins>00</ins>**3**<ins>0</ins>**, we get 123.

**Example 2:**

**Input:** n = 1

**Output:** 1

**Explanation:**

1 has no zero in its decimal representation. Therefore, the answer is 1.

**Constraints:**

*   <code>1 <= n <= 10<sup>15</sup></code>

## Solution

```java
public class Solution {
    public long removeZeros(long n) {
        StringBuilder x = new StringBuilder();
        String s = Long.toString(n);
        for (int i = 0; i < s.length(); i++) {
            if (s.charAt(i) != '0') {
                x.append(s.charAt(i));
            }
        }
        return Long.parseLong(x.toString());
    }
}
```