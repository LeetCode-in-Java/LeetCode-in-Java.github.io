[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3090\. Maximum Length Substring With Two Occurrences

Easy

Given a string `s`, return the **maximum** length of a substring such that it contains _at most two occurrences_ of each character.

**Example 1:**

**Input:** s = "bcbbbcba"

**Output:** 4

**Explanation:**

The following substring has a length of 4 and contains at most two occurrences of each character: <code>"bcbb<ins>bcba</ins>"</code>.

**Example 2:**

**Input:** s = "aaaa"

**Output:** 2

**Explanation:**

The following substring has a length of 2 and contains at most two occurrences of each character: <code>"<ins>aa</ins>aa"</code>.

**Constraints:**

*   `2 <= s.length <= 100`
*   `s` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    public int maximumLengthSubstring(String s) {
        int[] freq = new int[26];
        char[] chars = s.toCharArray();
        int i = 0;
        int len = s.length();
        int max = 0;
        for (int j = 0; j < len; j++) {
            ++freq[chars[j] - 'a'];
            while (freq[chars[j] - 'a'] == 3) {
                --freq[chars[i] - 'a'];
                i++;
            }
            max = Math.max(max, j - i + 1);
        }
        return max;
    }
}
```