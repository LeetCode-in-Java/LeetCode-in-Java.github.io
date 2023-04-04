[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 890\. Find and Replace Pattern

Medium

Given a list of strings `words` and a string `pattern`, return _a list of_ `words[i]` _that match_ `pattern`. You may return the answer in **any order**.

A word matches the pattern if there exists a permutation of letters `p` so that after replacing every letter `x` in the pattern with `p(x)`, we get the desired word.

Recall that a permutation of letters is a bijection from letters to letters: every letter maps to another letter, and no two letters map to the same letter.

**Example 1:**

**Input:** words = ["abc","deq","mee","aqq","dkd","ccc"], pattern = "abb"

**Output:** ["mee","aqq"]

**Explanation:** "mee" matches the pattern because there is a permutation {a -> m, b -> e, ...}. "ccc" does not match the pattern because {a -> c, b -> c, ...} is not a permutation, since a and b map to the same letter.

**Example 2:**

**Input:** words = ["a","b","c"], pattern = "a"

**Output:** ["a","b","c"]

**Constraints:**

*   `1 <= pattern.length <= 20`
*   `1 <= words.length <= 50`
*   `words[i].length == pattern.length`
*   `pattern` and `words[i]` are lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.Collections;
import java.util.HashMap;
import java.util.List;

@SuppressWarnings("java:S135")
public class Solution {
    public List<String> findAndReplacePattern(String[] words, String pattern) {
        List<String> finalans = new ArrayList<>();
        if (pattern.length() == 1) {
            Collections.addAll(finalans, words);
            return finalans;
        }
        for (String word : words) {
            char[] check = new char[26];
            Arrays.fill(check, '1');
            HashMap<Character, Character> ans = new HashMap<>();
            for (int j = 0; j < word.length(); j++) {
                char pat = pattern.charAt(j);
                char wor = word.charAt(j);
                if (ans.containsKey(pat)) {
                    if (ans.get(pat) == wor) {
                        if (j == word.length() - 1) {
                            finalans.add(word);
                        }
                    } else {
                        break;
                    }
                } else {
                    if (j == word.length() - 1 && check[wor - 'a'] == '1') {
                        finalans.add(word);
                    }
                    if (check[wor - 'a'] != '1' && check[wor - 'a'] != pat) {
                        break;
                    }
                    if (check[wor - 'a'] == '1') {
                        ans.put(pat, wor);
                        check[wor - 'a'] = pat;
                    }
                }
            }
        }
        return finalans;
    }
}
```