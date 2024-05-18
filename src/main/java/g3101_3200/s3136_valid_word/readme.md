[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3136\. Valid Word

Easy

A word is considered **valid** if:

*   It contains a **minimum** of 3 characters.
*   It contains only digits (0-9), and English letters (uppercase and lowercase).
*   It includes **at least** one **vowel**.
*   It includes **at least** one **consonant**.

You are given a string `word`.

Return `true` if `word` is valid, otherwise, return `false`.

**Notes:**

*   `'a'`, `'e'`, `'i'`, `'o'`, `'u'`, and their uppercases are **vowels**.
*   A **consonant** is an English letter that is not a vowel.

**Example 1:**

**Input:** word = "234Adas"

**Output:** true

**Explanation:**

This word satisfies the conditions.

**Example 2:**

**Input:** word = "b3"

**Output:** false

**Explanation:**

The length of this word is fewer than 3, and does not have a vowel.

**Example 3:**

**Input:** word = "a3$e"

**Output:** false

**Explanation:**

This word contains a `'$'` character and does not have a consonant.

**Constraints:**

*   `1 <= word.length <= 20`
*   `word` consists of English uppercase and lowercase letters, digits, `'@'`, `'#'`, and `'$'`.

## Solution

```java
public class Solution {
    public boolean isValid(String word) {
        if (word.length() < 3) {
            return false;
        }
        if (word.contains("@") || word.contains("#") || word.contains("$")) {
            return false;
        }
        char[] vowels = {'a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'};
        char[] consonants = {
            'b', 'c', 'd', 'f', 'g', 'h', 'j', 'k', 'l', 'm', 'n', 'p', 'q', 'r', 's', 't', 'v',
            'w', 'x', 'y', 'z', 'B', 'C', 'D', 'F', 'G', 'H', 'J', 'K', 'L', 'M', 'N', 'P', 'Q',
            'R', 'S', 'T', 'V', 'W', 'X', 'Y', 'Z'
        };
        boolean flag1 = false;
        boolean flag2 = false;
        for (char c : vowels) {
            if (word.indexOf(c) != -1) {
                flag1 = true;
                break;
            }
        }
        for (char c : consonants) {
            if (word.indexOf(c) != -1) {
                flag2 = true;
                break;
            }
        }
        return flag1 && flag2;
    }
}
```