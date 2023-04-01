[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1684\. Count the Number of Consistent Strings

Easy

You are given a string `allowed` consisting of **distinct** characters and an array of strings `words`. A string is **consistent** if all characters in the string appear in the string `allowed`.

Return _the number of **consistent** strings in the array_ `words`.

**Example 1:**

**Input:** allowed = "ab", words = ["ad","bd","aaab","baa","badab"]

**Output:** 2

**Explanation:** Strings "aaab" and "baa" are consistent since they only contain characters 'a' and 'b'.

**Example 2:**

**Input:** allowed = "abc", words = ["a","b","c","ab","ac","bc","abc"]

**Output:** 7

**Explanation:** All strings are consistent.

**Example 3:**

**Input:** allowed = "cad", words = ["cc","acd","b","ba","bac","bad","ac","d"]

**Output:** 4

**Explanation:** Strings "cc", "acd", "ac", and "d" are consistent.

**Constraints:**

*   <code>1 <= words.length <= 10<sup>4</sup></code>
*   `1 <= allowed.length <= 26`
*   `1 <= words[i].length <= 10`
*   The characters in `allowed` are **distinct**.
*   `words[i]` and `allowed` contain only lowercase English letters.

## Solution

```java
public class Solution {
    public int countConsistentStrings(String allowed, String[] words) {
        int alwd = 0;
        int res = 0;
        for (int i = 0; i < allowed.length(); ++i) {
            alwd |= 1 << allowed.charAt(i);
        }
        for (String word : words) {
            int b = 0;
            for (int j = 0; j < word.length(); ++j) {
                b |= 1 << word.charAt(j);
            }
            if ((alwd | b) == alwd) {
                ++res;
            }
        }
        return res;
    }
}
```