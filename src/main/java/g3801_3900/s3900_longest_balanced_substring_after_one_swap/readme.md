[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3900\. Longest Balanced Substring After One Swap

Medium

You are given a binary string `s` consisting only of characters `'0'` and `'1'`.

A string is **balanced** if it contains an **equal** number of `'0'`s and `'1'`s.

You can perform **at most one** swap between any two characters in `s`. Then, you select a **balanced** substring from `s`.

Return an integer representing the **maximum** length of the **balanced** substring you can select.

**Example 1:**

**Input:** s = "100001"

**Output:** 4

**Explanation:**

*   Swap <code>"10<ins>**0**</ins>00<ins>**1**</ins>"</code>. The string becomes `"101000"`.
*   Select the substring <code>"<ins>**1010**</ins>00"</code>, which is balanced because it has two `'0'`s and two `'1'`s.

**Example 2:**

**Input:** s = "111"

**Output:** 0

**Explanation:**

*   Choose not to perform any swaps.
*   Select the empty substring, which is balanced because it has zero `'0'`s and zero `'1'`s.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists only of the characters `'0'` and `'1'`.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int longestBalanced(String s) {
        char[] arr = s.toCharArray();
        int n = arr.length;
        int bal = n + 1;
        int ans = 0;
        int[] nextIndex = new int[n];
        int[] balIndex = new int[2 * n + 3];
        Arrays.fill(balIndex, n + 1);
        for (int i = n - 1; i >= 0; i--) {
            bal += (('0' ^ arr[i]) << 1) - 1;
            nextIndex[i] = balIndex[bal];
            balIndex[bal] = i;
        }
        if (bal == n + 1) {
            return n;
        }
        int zeros = (2 * n + 1 - bal) / 2;
        int maxLength = 2 * Math.min(zeros, n - zeros);
        for (int i = 1; i <= n && ans < maxLength; i++) {
            bal += (('1' ^ arr[i - 1]) << 1) - 1;
            if (i - balIndex[bal] > ans) {
                ans = i - balIndex[bal];
            }
            if (balIndex[bal - 2] < i - maxLength) {
                balIndex[bal - 2] = nextIndex[balIndex[bal - 2]];
            }
            if (i - balIndex[bal - 2] > ans) {
                ans = i - balIndex[bal - 2];
            }
            if (balIndex[bal + 2] < i - maxLength) {
                balIndex[bal + 2] = nextIndex[balIndex[bal + 2]];
            }
            if (i - balIndex[bal + 2] > ans) {
                ans = i - balIndex[bal + 2];
            }
        }
        return ans;
    }
}
```