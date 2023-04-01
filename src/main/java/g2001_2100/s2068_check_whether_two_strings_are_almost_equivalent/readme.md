[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2068\. Check Whether Two Strings are Almost Equivalent

Easy

Two strings `word1` and `word2` are considered **almost equivalent** if the differences between the frequencies of each letter from `'a'` to `'z'` between `word1` and `word2` is **at most** `3`.

Given two strings `word1` and `word2`, each of length `n`, return `true` _if_ `word1` _and_ `word2` _are **almost equivalent**, or_ `false` _otherwise_.

The **frequency** of a letter `x` is the number of times it occurs in the string.

**Example 1:**

**Input:** word1 = "aaaa", word2 = "bccb"

**Output:** false

**Explanation:** There are 4 'a's in "aaaa" but 0 'a's in "bccb". 

The difference is 4, which is more than the allowed 3. 

**Example 2:**

**Input:** word1 = "abcdeef", word2 = "abaaacc"

**Output:** true

**Explanation:** The differences between the frequencies of each letter in word1 and word2 are at most 3:
 
- 'a' appears 1 time in word1 and 4 times in word2. The difference is 3.

- 'b' appears 1 time in word1 and 1 time in word2. The difference is 0.

- 'c' appears 1 time in word1 and 2 times in word2. The difference is 1.

- 'd' appears 1 time in word1 and 0 times in word2. The difference is 1.

- 'e' appears 2 times in word1 and 0 times in word2. The difference is 2.

- 'f' appears 1 time in word1 and 0 times in word2. The difference is 1. 

**Example 3:**

**Input:** word1 = "cccddabba", word2 = "babababab"

**Output:** true

**Explanation:** The differences between the frequencies of each letter in word1 and word2 are at most 3:

- 'a' appears 2 times in word1 and 4 times in word2. The difference is 2.

- 'b' appears 2 times in word1 and 5 times in word2. The difference is 3.

- 'c' appears 3 times in word1 and 0 times in word2. The difference is 3.

- 'd' appears 2 times in word1 and 0 times in word2. The difference is 2. 

**Constraints:**

*   `n == word1.length == word2.length`
*   `1 <= n <= 100`
*   `word1` and `word2` consist only of lowercase English letters.

## Solution

```java
public class Solution {
    public boolean checkAlmostEquivalent(String word1, String word2) {
        int[] freq = new int[26];
        for (int i = 0; i < word1.length(); i++) {
            ++freq[word1.charAt(i) - 'a'];
            --freq[word2.charAt(i) - 'a'];
        }
        for (int i : freq) {
            if (Math.abs(i) > 3) {
                return false;
            }
        }
        return true;
    }
}
```