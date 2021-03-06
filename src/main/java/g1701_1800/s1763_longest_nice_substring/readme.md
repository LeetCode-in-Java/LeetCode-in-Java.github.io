[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1763\. Longest Nice Substring

Easy

A string `s` is **nice** if, for every letter of the alphabet that `s` contains, it appears **both** in uppercase and lowercase. For example, `"abABB"` is nice because `'A'` and `'a'` appear, and `'B'` and `'b'` appear. However, `"abA"` is not because `'b'` appears, but `'B'` does not.

Given a string `s`, return _the longest **substring** of `s` that is **nice**. If there are multiple, return the substring of the **earliest** occurrence. If there are none, return an empty string_.

**Example 1:**

**Input:** s = "YazaAay"

**Output:** "aAa"

**Explanation:** "aAa" is a nice string because 'A/a' is the only letter of the alphabet in s, and both 'A' and 'a' appear. "aAa" is the longest nice substring.

**Example 2:**

**Input:** s = "Bb"

**Output:** "Bb"

**Explanation:** "Bb" is a nice string because both 'B' and 'b' appear. The whole string is a substring.

**Example 3:**

**Input:** s = "c"

**Output:** ""

**Explanation:** There are no nice substrings.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of uppercase and lowercase English letters.

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public String longestNiceSubstring(String s) {
        int index = isNotNiceString(s);
        if (index == -1) {
            return s;
        }
        String left = longestNiceSubstring(s.substring(0, index));
        String right = longestNiceSubstring(s.substring(index + 1));
        return left.length() >= right.length() ? left : right;
    }

    private int isNotNiceString(String s) {
        Set<Character> set = new HashSet<>();
        for (char c : s.toCharArray()) {
            set.add(c);
        }
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (!set.contains(Character.toLowerCase(c))
                    || !set.contains(Character.toUpperCase(c))) {
                return i;
            }
        }
        return -1;
    }
}
```