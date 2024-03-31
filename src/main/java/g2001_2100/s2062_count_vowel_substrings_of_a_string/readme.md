[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2062\. Count Vowel Substrings of a String

Easy

A **substring** is a contiguous (non-empty) sequence of characters within a string.

A **vowel substring** is a substring that **only** consists of vowels (`'a'`, `'e'`, `'i'`, `'o'`, and `'u'`) and has **all five** vowels present in it.

Given a string `word`, return _the number of **vowel substrings** in_ `word`.

**Example 1:**

**Input:** word = "aeiouu"

**Output:** 2

**Explanation:** The vowel substrings of word are as follows (underlined): 

- "**aeiou**u" 

- "**aeiouu**"

**Example 2:**

**Input:** word = "unicornarihan"

**Output:** 0

**Explanation:** Not all 5 vowels are present, so there are no vowel substrings.

**Example 3:**

**Input:** word = "cuaieuouac"

**Output:** 7

**Explanation:** The vowel substrings of word are as follows (underlined): 

- "c**uaieuo**uac" 

- "c**uaieuou**ac" 

- "c**uaieuoua**c" 

- "cu**aieuo**uac" 

- "cu**aieuou**ac" 

- "cu**aieuoua**c" 

- "cua**ieuoua**c"

**Constraints:**

*   `1 <= word.length <= 100`
*   `word` consists of lowercase English letters only.

## Solution

```java
public class Solution {
    public int countVowelSubstrings(String word) {
        final int length = word.length();
        boolean[] vows = new boolean[128];
        vows['a'] = true;
        vows['e'] = true;
        vows['i'] = true;
        vows['o'] = true;
        vows['u'] = true;
        int[] counts = new int[128];
        int uniqVows = 0;
        int originalBegin = 0;
        int begin = 0;
        int result = 0;
        for (int i = 0; i < length; i++) {
            char ch = word.charAt(i);
            if (vows[ch]) {
                counts[ch]++;
                if (counts[ch] == 1) {
                    uniqVows++;
                }
                while (uniqVows == 5) {
                    uniqVows -= --counts[word.charAt(begin)] == 0 ? 1 : 0;
                    begin++;
                }
                result += begin - originalBegin;
            } else {
                if (uniqVows != 0) {
                    uniqVows = 0;
                    counts['a'] = 0;
                    counts['e'] = 0;
                    counts['i'] = 0;
                    counts['o'] = 0;
                    counts['u'] = 0;
                }
                originalBegin = begin = i + 1;
            }
        }
        return result;
    }
}
```