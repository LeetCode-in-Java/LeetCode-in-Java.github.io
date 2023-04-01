[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 345\. Reverse Vowels of a String

Easy

Given a string `s`, reverse only all the vowels in the string and return it.

The vowels are `'a'`, `'e'`, `'i'`, `'o'`, and `'u'`, and they can appear in both cases.

**Example 1:**

**Input:** s = "hello"

**Output:** "holle"

**Example 2:**

**Input:** s = "leetcode"

**Output:** "leotcede"

**Constraints:**

*   1 <= s.length <= 3 * 105
*   `s` consist of **printable ASCII** characters.

## Solution

```java
public class Solution {
    private boolean isVowel(char c) {
        return c == 'a' || c == 'A' || c == 'e' || c == 'E' || c == 'i' || c == 'I' || c == 'o'
                || c == 'O' || c == 'u' || c == 'U';
    }

    public String reverseVowels(String str) {
        int i = 0;
        int j = str.length() - 1;
        char[] str1 = str.toCharArray();
        while (i < j) {
            if (!isVowel(str1[i])) {
                i++;
            } else if (!isVowel(str1[j])) {
                j--;
            } else {
                // swapping
                char t = str1[i];
                str1[i] = str1[j];
                str1[j] = t;
                i++;
                j--;
            }
        }
        return String.copyValueOf(str1);
    }
}
```