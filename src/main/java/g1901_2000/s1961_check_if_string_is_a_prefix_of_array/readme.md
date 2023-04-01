[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1961\. Check If String Is a Prefix of Array

Easy

Given a string `s` and an array of strings `words`, determine whether `s` is a **prefix string** of `words`.

A string `s` is a **prefix string** of `words` if `s` can be made by concatenating the first `k` strings in `words` for some **positive** `k` no larger than `words.length`.

Return `true` _if_ `s` _is a **prefix string** of_ `words`_, or_ `false` _otherwise_.

**Example 1:**

**Input:** s = "iloveleetcode", words = ["i","love","leetcode","apples"]

**Output:** true

**Explanation:** s can be made by concatenating "i", "love", and "leetcode" together.

**Example 2:**

**Input:** s = "iloveleetcode", words = ["apples","i","love","leetcode"]

**Output:** false

**Explanation:** It is impossible to make s using a prefix of arr.

**Constraints:**

*   `1 <= words.length <= 100`
*   `1 <= words[i].length <= 20`
*   `1 <= s.length <= 1000`
*   `words[i]` and `s` consist of only lowercase English letters.

## Solution

```java
public class Solution {
    public boolean isPrefixString(String s, String[] words) {
        StringBuilder sb = new StringBuilder();
        for (String word : words) {
            sb.append(word);
            if (sb.toString().equals(s)) {
                return true;
            }
        }
        return false;
    }
}
```