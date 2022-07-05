[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 792\. Number of Matching Subsequences

Medium

Given a string `s` and an array of strings `words`, return _the number of_ `words[i]` _that is a subsequence of_ `s`.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

*   For example, `"ace"` is a subsequence of `"abcde"`.

**Example 1:**

**Input:** s = "abcde", words = ["a","bb","acd","ace"]

**Output:** 3

**Explanation:** There are three strings in words that are a subsequence of s: "a", "acd", "ace". 

**Example 2:**

**Input:** s = "dsahjpjauf", words = ["ahjpjau","ja","ahbwzgqnuk","tnmlanowax"]

**Output:** 2 

**Constraints:**

*   <code>1 <= s.length <= 5 * 10<sup>4</sup></code>
*   `1 <= words.length <= 5000`
*   `1 <= words[i].length <= 50`
*   `s` and `words[i]` consist of only lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("unchecked")
public class Solution {
    public int numMatchingSubseq(String s, String[] words) {
        List<Node>[] buckets = new ArrayList[26];
        for (int i = 0; i < buckets.length; i++) {
            buckets[i] = new ArrayList<>();
        }
        for (String word : words) {
            char start = word.charAt(0);
            buckets[start - 'a'].add(new Node(word, 0));
        }
        int result = 0;
        for (char c : s.toCharArray()) {
            List<Node> currBucket = buckets[c - 'a'];
            buckets[c - 'a'] = new ArrayList<>();
            for (Node node : currBucket) {
                node.index++;
                if (node.index == node.word.length()) {
                    result++;
                } else {
                    char start = node.word.charAt(node.index);
                    buckets[start - 'a'].add(node);
                }
            }
        }
        return result;
    }

    private static class Node {
        String word;
        int index;

        public Node(String word, int index) {
            this.word = word;
            this.index = index;
        }
    }
}
```