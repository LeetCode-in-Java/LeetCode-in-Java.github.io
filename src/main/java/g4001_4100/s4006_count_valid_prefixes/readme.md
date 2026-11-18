[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4006\. Count Valid Prefixes

Easy

You are given a binary string `s`.

A prefix of `s` is considered **valid** if its characters can be rearranged to form an **alternating** string.

Return the number of valid prefixes of `s`.

A string is considered **alternating** if no two adjacent characters are equal.

**Example 1:**

**Input:** s = "00101"

**Output:** 3

**Explanation:**

The valid prefixes are:

*   `"0"`: It is already an alternating string.
*   `"001"`: It can be rearranged into `"010"`, which is an alternating string.
*   `"00101"`: It can be rearranged into `"01010"`, which is an alternating string.

Thus, the answer is 3.

**Example 2:**

**Input:** s = "101"

**Output:** 3

**Explanation:**

All prefixes of `s = "101"` are already alternating strings. Thus, the answer is 3.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists only of `'0'` and `'1'`.

## Solution

```java
public class Solution {
    public int countValidPrefixes(String s) {
        int one = 0;
        int zero = 0;
        int ans = 0;
        for (char c : s.toCharArray()) {
            if (c == '1') {
                one++;
            } else {
                zero++;
            }
            if (Math.abs(one - zero) <= 1) {
                ans++;
            }
        }
        return ans;
    }
}
```