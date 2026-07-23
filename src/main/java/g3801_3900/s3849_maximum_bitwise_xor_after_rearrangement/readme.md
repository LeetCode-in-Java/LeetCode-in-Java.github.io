[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3849\. Maximum Bitwise XOR After Rearrangement

Medium

You are given two binary strings `s` and `t`, each of length `n`.

You may **rearrange** the characters of `t` in any order, but `s` **must remain unchanged**.

Return a **binary string** of length `n` representing the **maximum** integer value obtainable by taking the bitwise **XOR** of `s` and rearranged `t`.

**Example 1:**

**Input:** s = "101", t = "011"

**Output:** "110"

**Explanation:**

*   One optimal rearrangement of `t` is `"011"`.
*   The bitwise XOR of `s` and rearranged `t` is `"101" XOR "011" = "110"`, which is the maximum possible.

**Example 2:**

**Input:** s = "0110", t = "1110"

**Output:** "1101"

**Explanation:**

*   One optimal rearrangement of `t` is `"1011"`.
*   The bitwise XOR of `s` and rearranged `t` is `"0110" XOR "1011" = "1101"`, which is the maximum possible.

**Example 3:**

**Input:** s = "0101", t = "1001"

**Output:** "1111"

**Explanation:**

*   One optimal rearrangement of `t` is `"1010"`.
*   The bitwise XOR of `s` and rearranged `t` is `"0101" XOR "1010" = "1111"`, which is the maximum possible.

**Constraints:**

*   <code>1 <= n == s.length == t.length <= 2 * 10<sup>5</sup></code>
*   `s[i]` and `t[i]` are either `'0'` or `'1'`.

## Solution

```java
public class Solution {
    public String maximumXor(String s, String t) {
        int[] cnt = new int[2];
        for (char c : t.toCharArray()) {
            cnt[c - '0']++;
        }

        char[] ans = new char[s.length()];
        for (int i = 0; i < s.length(); i++) {
            int x = s.charAt(i) - '0';
            if (cnt[x ^ 1] > 0) {
                cnt[x ^ 1]--;
                ans[i] = '1';
            } else {
                cnt[x]--;
                ans[i] = '0';
            }
        }

        return new String(ans);
    }
}
```