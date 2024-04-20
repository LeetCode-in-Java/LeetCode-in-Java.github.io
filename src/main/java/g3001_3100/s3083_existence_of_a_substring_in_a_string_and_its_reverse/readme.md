[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3083\. Existence of a Substring in a String and Its Reverse

Easy

Given a string `s`, find any substring of length `2` which is also present in the reverse of `s`.

Return `true` _if such a substring exists, and_ `false` _otherwise._

**Example 1:**

**Input:** s = "leetcode"

**Output:** true

**Explanation:** Substring `"ee"` is of length `2` which is also present in `reverse(s) == "edocteel"`.

**Example 2:**

**Input:** s = "abcba"

**Output:** true

**Explanation:** All of the substrings of length `2` `"ab"`, `"bc"`, `"cb"`, `"ba"` are also present in `reverse(s) == "abcba"`.

**Example 3:**

**Input:** s = "abcd"

**Output:** false

**Explanation:** There is no substring of length `2` in `s`, which is also present in the reverse of `s`.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    public boolean isSubstringPresent(String s) {
        if (s.length() == 1) {
            return false;
        }
        StringBuilder revSb = new StringBuilder();
        for (int i = s.length() - 1; i >= 0; i--) {
            revSb.append(s.charAt(i));
        }
        String rev = revSb.toString();
        for (int i = 0; i < s.length() - 1; i++) {
            String sub = s.substring(i, i + 2);
            if (rev.contains(sub)) {
                return true;
            }
        }
        return false;
    }
}
```