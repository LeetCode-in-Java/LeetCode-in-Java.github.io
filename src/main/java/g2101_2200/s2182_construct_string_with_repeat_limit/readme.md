[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2182\. Construct String With Repeat Limit

Medium

You are given a string `s` and an integer `repeatLimit`. Construct a new string `repeatLimitedString` using the characters of `s` such that no letter appears **more than** `repeatLimit` times **in a row**. You do **not** have to use all characters from `s`.

Return _the **lexicographically largest**_ `repeatLimitedString` _possible_.

A string `a` is **lexicographically larger** than a string `b` if in the first position where `a` and `b` differ, string `a` has a letter that appears later in the alphabet than the corresponding letter in `b`. If the first `min(a.length, b.length)` characters do not differ, then the longer string is the lexicographically larger one.

**Example 1:**

**Input:** s = "cczazcc", repeatLimit = 3

**Output:** "zzcccac"

**Explanation:** We use all of the characters from s to construct the repeatLimitedString "zzcccac".

The letter 'a' appears at most 1 time in a row.

The letter 'c' appears at most 3 times in a row.

The letter 'z' appears at most 2 times in a row.

Hence, no letter appears more than repeatLimit times in a row and the string is a valid repeatLimitedString.

The string is the lexicographically largest repeatLimitedString possible so we return "zzcccac".

Note that the string "zzcccca" is lexicographically larger but the letter 'c' appears more than 3 times in a row, so it is not a valid repeatLimitedString. 

**Example 2:**

**Input:** s = "aababab", repeatLimit = 2

**Output:** "bbabaa"

**Explanation:** We use only some of the characters from s to construct the repeatLimitedString "bbabaa".

The letter 'a' appears at most 2 times in a row. The letter 'b' appears at most 2 times in a row.

Hence, no letter appears more than repeatLimit times in a row and the string is a valid repeatLimitedString.

The string is the lexicographically largest repeatLimitedString possible so we return "bbabaa".

Note that the string "bbabaaa" is lexicographically larger but the letter 'a' appears more than 2 times in a row, so it is not a valid repeatLimitedString. 

**Constraints:**

*   <code>1 <= repeatLimit <= s.length <= 10<sup>5</sup></code>
*   `s` consists of lowercase English letters.

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    public String repeatLimitedString(String s, int repeatLimit) {
        char[] result = new char[s.length()];
        int[] freq = new int[128];
        int index = 0;
        for (char c : s.toCharArray()) {
            freq[c]++;
        }
        char max = 'z';
        char second = 'y';
        while (true) {
            while (max >= 'a' && freq[max] == 0) {
                max--;
            }
            if (max < 'a') {
                break;
            }
            second = (char) Math.min(max - 1, second);
            int count = Math.min(freq[max], repeatLimit);
            freq[max] -= count;
            while (count-- > 0) {
                result[index++] = max;
            }
            if (freq[max] == 0) {
                max = second--;
                continue;
            }
            while (second >= 'a' && freq[second] == 0) {
                second--;
            }
            if (second < 'a') {
                break;
            }
            result[index++] = second;
            freq[second]--;
        }
        return new String(result, 0, index);
    }
}
```