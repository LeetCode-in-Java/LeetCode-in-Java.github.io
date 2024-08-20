[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3258\. Count Substrings That Satisfy K-Constraint I

Easy

You are given a **binary** string `s` and an integer `k`.

A **binary string** satisfies the **k-constraint** if **either** of the following conditions holds:

*   The number of `0`'s in the string is at most `k`.
*   The number of `1`'s in the string is at most `k`.

Return an integer denoting the number of substrings of `s` that satisfy the **k-constraint**.

**Example 1:**

**Input:** s = "10101", k = 1

**Output:** 12

**Explanation:**

Every substring of `s` except the substrings `"1010"`, `"10101"`, and `"0101"` satisfies the k-constraint.

**Example 2:**

**Input:** s = "1010101", k = 2

**Output:** 25

**Explanation:**

Every substring of `s` except the substrings with a length greater than 5 satisfies the k-constraint.

**Example 3:**

**Input:** s = "11111", k = 1

**Output:** 15

**Explanation:**

All substrings of `s` satisfy the k-constraint.

**Constraints:**

*   `1 <= s.length <= 50`
*   `1 <= k <= s.length`
*   `s[i]` is either `'0'` or `'1'`.

## Solution

```java
public class Solution {
    public int countKConstraintSubstrings(String s, int k) {
        int n = s.length();
        int sum = n;
        int i = 0;
        int j = 0;
        int one = 0;
        int zero = 0;
        char ch;
        while (j < n) {
            ch = s.charAt(j++);
            if (ch == '0') {
                zero++;
            } else {
                one++;
            }
            while (i <= j && one > k && zero > k) {
                ch = s.charAt(i++);
                if (ch == '0') {
                    zero--;
                } else {
                    one--;
                }
            }
            int len = (zero + one - 1);
            sum += len;
        }
        return sum;
    }
}
```