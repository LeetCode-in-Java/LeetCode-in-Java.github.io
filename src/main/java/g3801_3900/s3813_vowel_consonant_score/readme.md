[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3813\. Vowel-Consonant Score

Easy

You are given a string `s` consisting of lowercase English letters, spaces, and digits.

Let `v` be the number of vowels in `s` and `c` be the number of consonants in `s`.

A vowel is one of the letters `'a'`, `'e'`, `'i'`, `'o'`, or `'u'`, while any other letter in the English alphabet is considered a consonant.

The **score** of the string `s` is defined as follows:

*   If `c > 0`, the `score = floor(v / c)` where floor denotes **rounding down** to the nearest integer.
*   Otherwise, the `score = 0`.

Return an integer denoting the score of the string.

**Example 1:**

**Input:** s = "cooear"

**Output:** 2

**Explanation:**

The string `s = "cooear"` contains `v = 4` vowels `('o', 'o', 'e', 'a')` and `c = 2` consonants `('c', 'r')`.

The score is `floor(v / c) = floor(4 / 2) = 2`.

**Example 2:**

**Input:** s = "axeyizou"

**Output:** 1

**Explanation:**

The string `s = "axeyizou"` contains `v = 5` vowels `('a', 'e', 'i', 'o', 'u')` and `c = 3` consonants `('x', 'y', 'z')`.

The score is `floor(v / c) = floor(5 / 3) = 1`.

**Example 3:**

**Input:** s = "au 123"

**Output:** 0

**Explanation:**

The string `s = "au 123"` contains no consonants `(c = 0)`, so the score is 0.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of lowercase English letters, spaces and digits.

## Solution

```java
public class Solution {
    public int vowelConsonantScore(String s) {
        int count = 0;
        String vowels = "aeiou";
        int con = 0;
        for (char ch : s.toCharArray()) {
            if (vowels.indexOf(ch) != -1) {
                count++;
                continue;
            }
            if (Character.isAlphabetic(ch)) {
                con++;
            }
        }
        return con == 0 ? 0 : count / con;
    }
}
```