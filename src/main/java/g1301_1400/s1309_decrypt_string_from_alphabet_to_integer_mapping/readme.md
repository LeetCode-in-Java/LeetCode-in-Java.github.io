[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1309\. Decrypt String from Alphabet to Integer Mapping

Easy

You are given a string `s` formed by digits and `'#'`. We want to map `s` to English lowercase characters as follows:

*   Characters (`'a'` to `'i')` are represented by (`'1'` to `'9'`) respectively.
*   Characters (`'j'` to `'z')` are represented by (`'10#'` to `'26#'`) respectively.

Return _the string formed after mapping_.

The test cases are generated so that a unique mapping will always exist.

**Example 1:**

**Input:** s = "10#11#12"

**Output:** "jkab"

**Explanation:** "j" -> "10#" , "k" -> "11#" , "a" -> "1" , "b" -> "2".

**Example 2:**

**Input:** s = "1326#"

**Output:** "acz"

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consists of digits and the `'#'` letter.
*   `s` will be a valid string such that mapping is always possible.

## Solution

```java
public class Solution {
    public String freqAlphabets(String s) {
        StringBuilder builder = new StringBuilder();
        int i = s.length() - 1;
        while (i >= 0) {
            if (s.charAt(i) == '#') {
                decryptor(builder, i - 1, i - 2, s);
                i -= 3;
            } else {
                char ch = (char) (s.charAt(i) - '0' + 96);
                builder.append(ch);
                i--;
            }
        }
        return builder.reverse().toString();
    }

    private void decryptor(StringBuilder builder, int a, int b, String s) {
        builder.append((char) (((s.charAt(b) - '0') * 10 + s.charAt(a) - '0') + 96));
    }
}
```