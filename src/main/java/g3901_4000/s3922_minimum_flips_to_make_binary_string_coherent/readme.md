[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3922\. Minimum Flips to Make Binary String Coherent

Medium

You are given a binary string `s`.

A string is considered **coherent** if it does **not** contain `"011"` or `"110"` as subsequences.

In one operation, you can **flip** any character in `s` (`'0'` to `'1'` or `'1'` to `'0'`).

Return an integer denoting the **minimum** number of operations required to make `s` coherent.

**Example 1:**

**Input:** s = "1010"

**Output:** 1

**Explanation:**

Flip `s[0]` to get `"0010"`, which contains no `"011"` or `"110"` subsequences.

**Example 2:**

**Input:** s = "0110"

**Output:** 1

**Explanation:**

Flip `s[1]` to get `"0010"`, removing all forbidden subsequences `"011"` and `"110"`.

**Example 3:**

**Input:** s = "1000"

**Output:** 0

**Explanation:**

The string already has no `"011"` or `"110"` subsequences, so no flips are needed.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`.

## Solution

```java
public class Solution {
    public int minFlips(String s) {
        int n = s.length();
        int count1 = 0;
        for (char c : s.toCharArray()) {
            count1 += c - '0';
        }
        int count0 = n - count1;
        // Option 1: all 0s
        int ans = count1;
        // Option 2: all 1s
        ans = Math.min(ans, count0);
        // Option 3: exactly one 1 (at any position)
        if (count1 >= 1) {
            ans = Math.min(ans, count1 - 1);
        } else {
            ans = Math.min(ans, 1);
        }
        // Option 4: "10...01" (1 at position 0 and position n-1)
        if (n >= 2 && count1 >= 2) {
            int cost4 = count1;
            if (s.charAt(0) == '1') {
                cost4--;
            }
            if (s.charAt(n - 1) == '1') {
                cost4--;
            }
            ans = Math.min(ans, cost4);
        }
        return ans;
    }
}
```