## 1839\. Longest Substring Of All Vowels in Order

Medium

A string is considered **beautiful** if it satisfies the following conditions:

*   Each of the 5 English vowels (`'a'`, `'e'`, `'i'`, `'o'`, `'u'`) must appear **at least once** in it.
*   The letters must be sorted in **alphabetical order** (i.e. all `'a'`s before `'e'`s, all `'e'`s before `'i'`s, etc.).

For example, strings `"aeiou"` and `"aaaaaaeiiiioou"` are considered **beautiful**, but `"uaeio"`, `"aeoiu"`, and `"aaaeeeooo"` are **not beautiful**.

Given a string `word` consisting of English vowels, return _the **length of the longest beautiful substring** of_ `word`_. If no such substring exists, return_ `0`.

A **substring** is a contiguous sequence of characters in a string.

**Example 1:**

**Input:** word = "aeiaaioaaaaeiiiiouuuooaauuaeiu"

**Output:** 13

**Explanation:** The longest beautiful substring in word is "aaaaeiiiiouuu" of length 13.

**Example 2:**

**Input:** word = "aeeeiiiioooauuuaeiou"

**Output:** 5

**Explanation:** The longest beautiful substring in word is "aeiou" of length 5.

**Example 3:**

**Input:** word = "a"

**Output:** 0

**Explanation:** There is no beautiful substring, so return 0.

**Constraints:**

*   <code>1 <= word.length <= 5 * 10<sup>5</sup></code>
*   `word` consists of characters `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`.

## Solution

```java
public class Solution {
    public int longestBeautifulSubstring(String word) {
        int cnt = 1;
        int len = 1;
        int maxLen = 0;
        for (int i = 1; i != word.length(); ++i) {
            if (word.charAt(i - 1) == word.charAt(i)) {
                ++len;
            } else if (word.charAt(i - 1) < word.charAt(i)) {
                ++len;
                ++cnt;
            } else {
                cnt = 1;
                len = 1;
            }

            if (cnt == 5) {
                maxLen = Math.max(maxLen, len);
            }
        }
        return maxLen;
    }
}
```