[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1668\. Maximum Repeating Substring

Easy

For a string `sequence`, a string `word` is **`k`\-repeating** if `word` concatenated `k` times is a substring of `sequence`. The `word`'s **maximum `k`\-repeating value** is the highest value `k` where `word` is `k`\-repeating in `sequence`. If `word` is not a substring of `sequence`, `word`'s maximum `k`\-repeating value is `0`.

Given strings `sequence` and `word`, return _the **maximum `k`\-repeating value** of `word` in `sequence`_.

**Example 1:**

**Input:** sequence = "ababc", word = "ab"

**Output:** 2

**Explanation:** "abab" is a substring in "ababc".

**Example 2:**

**Input:** sequence = "ababc", word = "ba"

**Output:** 1

**Explanation:** "ba" is a substring in "ababc". "baba" is not a substring in "ababc".

**Example 3:**

**Input:** sequence = "ababc", word = "ac"

**Output:** 0

**Explanation:** "ac" is not a substring in "ababc".

**Constraints:**

*   `1 <= sequence.length <= 100`
*   `1 <= word.length <= 100`
*   `sequence` and `word` contains only lowercase English letters.

## Solution

```java
public class Solution {
    public int maxRepeating(String sequence, String word) {
        int k = 0;
        StringBuilder repeat = new StringBuilder(word);
        while (sequence.contains(repeat)) {
            k++;
            repeat.append(word);
        }
        return k;
    }
}
```