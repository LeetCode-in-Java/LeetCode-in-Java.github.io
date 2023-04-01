[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1662\. Check If Two String Arrays are Equivalent

Easy

Given two string arrays `word1` and `word2`, return `true` _if the two arrays **represent** the same string, and_ `false` _otherwise._

A string is **represented** by an array if the array elements concatenated **in order** forms the string.

**Example 1:**

**Input:** word1 = ["ab", "c"], word2 = ["a", "bc"]

**Output:** true

**Explanation:**

word1 represents string "ab" + "c" -> "abc"

word2 represents string "a" + "bc" -> "abc"

The strings are the same, so return true.

**Example 2:**

**Input:** word1 = ["a", "cb"], word2 = ["ab", "c"]

**Output:** false

**Example 3:**

**Input:** word1 = ["abc", "d", "defg"], word2 = ["abcddefg"]

**Output:** true

**Constraints:**

*   <code>1 <= word1.length, word2.length <= 10<sup>3</sup></code>
*   <code>1 <= word1[i].length, word2[i].length <= 10<sup>3</sup></code>
*   <code>1 <= sum(word1[i].length), sum(word2[i].length) <= 10<sup>3</sup></code>
*   `word1[i]` and `word2[i]` consist of lowercase letters.

## Solution

```java
public class Solution {
    public boolean arrayStringsAreEqual(String[] word1, String[] word2) {
        StringBuilder sb1 = new StringBuilder();
        for (String word : word1) {
            sb1.append(word);
        }
        StringBuilder sb2 = new StringBuilder();
        for (String word : word2) {
            sb2.append(word);
        }
        return sb1.toString().equals(sb2.toString());
    }
}
```