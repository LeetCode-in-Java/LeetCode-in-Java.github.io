[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 318\. Maximum Product of Word Lengths

Medium

Given a string array `words`, return _the maximum value of_ `length(word[i]) * length(word[j])` _where the two words do not share common letters_. If no such two words exist, return `0`.

**Example 1:**

**Input:** words = ["abcw","baz","foo","bar","xtfn","abcdef"]

**Output:** 16

**Explanation:** The two words can be "abcw", "xtfn". 

**Example 2:**

**Input:** words = ["a","ab","abc","d","cd","bcd","abcd"]

**Output:** 4

**Explanation:** The two words can be "ab", "cd". 

**Example 3:**

**Input:** words = ["a","aa","aaa","aaaa"]

**Output:** 0

**Explanation:** No such pair of words. 

**Constraints:**

*   `2 <= words.length <= 1000`
*   `1 <= words[i].length <= 1000`
*   `words[i]` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    public int maxProduct(String[] words) {
        int n = words.length;
        int res = 0;
        int[] masks = new int[n];
        for (int i = 0; i < n; i++) {
            masks[i] = getMask(words[i]);
        }
        for (int i = 0; i < n; i++) {
            for (int j = i + 1; j < n; j++) {
                if ((masks[i] & masks[j]) == 0) {
                    res = Math.max(res, words[i].length() * words[j].length());
                }
            }
        }
        return res;
    }

    private int getMask(String s) {
        int mask = 0;
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            mask = mask | (1 << (c - 'a'));
        }
        return mask;
    }
}
```