[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1704\. Determine if String Halves Are Alike

Easy

You are given a string `s` of even length. Split this string into two halves of equal lengths, and let `a` be the first half and `b` be the second half.

Two strings are **alike** if they have the same number of vowels (`'a'`, `'e'`, `'i'`, `'o'`, `'u'`, `'A'`, `'E'`, `'I'`, `'O'`, `'U'`). Notice that `s` contains uppercase and lowercase letters.

Return `true` _if_ `a` _and_ `b` _are **alike**_. Otherwise, return `false`.

**Example 1:**

**Input:** s = "book"

**Output:** true

**Explanation:** a = "bo" and b = "ok". a has 1 vowel and b has 1 vowel. Therefore, they are alike.

**Example 2:**

**Input:** s = "textbook"

**Output:** false

**Explanation:** a = "text" and b = "book". a has 1 vowel whereas b has 2. Therefore, they are not alike. Notice that the vowel o is counted twice.

**Constraints:**

*   `2 <= s.length <= 1000`
*   `s.length` is even.
*   `s` consists of **uppercase and lowercase** letters.

## Solution

```java
public class Solution {
    public boolean halvesAreAlike(String s) {
        if (s.isEmpty()) {
            return false;
        }
        return countVowel(0, s.length() / 2, s) == countVowel(s.length() / 2, s.length(), s);
    }

    private int countVowel(int start, int end, String s) {
        int c = 0;
        for (int i = start; i < end; i++) {
            char ch = s.charAt(i);
            if (ch == 'a' || ch == 'e' || ch == 'i' || ch == 'o' || ch == 'u' || ch == 'A'
                    || ch == 'E' || ch == 'I' || ch == 'O' || ch == 'U') {
                c++;
            }
        }
        return c;
    }
}
```