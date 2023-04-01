[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 720\. Longest Word in Dictionary

Medium

Given an array of strings `words` representing an English Dictionary, return _the longest word in_ `words` _that can be built one character at a time by other words in_ `words`.

If there is more than one possible answer, return the longest word with the smallest lexicographical order. If there is no answer, return the empty string.

**Example 1:**

**Input:** words = ["w","wo","wor","worl","world"]

**Output:** "world"

**Explanation:** The word "world" can be built one character at a time by "w", "wo", "wor", and "worl".

**Example 2:**

**Input:** words = ["a","banana","app","appl","ap","apply","apple"]

**Output:** "apple"

**Explanation:** Both "apply" and "apple" can be built from other words in the dictionary. However, "apple" is lexicographically smaller than "apply".

**Constraints:**

*   `1 <= words.length <= 1000`
*   `1 <= words[i].length <= 30`
*   `words[i]` consists of lowercase English letters.

## Solution

```java
public class Solution {
    private final Node root = new Node();
    private String longestWord = "";

    private static class Node {
        Node[] children;
        String str;

        public Node() {
            this.children = new Node[26];
            this.str = null;
        }

        public void insert(Node curr, String word) {
            for (int i = 0; i < word.length(); i++) {
                char ch = word.charAt(i);
                if (curr.children[ch - 'a'] == null) {
                    curr.children[ch - 'a'] = new Node();
                }
                curr = curr.children[ch - 'a'];
            }
            curr.str = word;
        }
    }

    public String longestWord(String[] words) {
        for (String word : words) {
            root.insert(root, word);
        }
        dfs(root);
        return longestWord;
    }

    private void dfs(Node curr) {
        for (int i = 0; i < 26; i++) {
            if (curr.children[i] != null && curr.children[i].str != null) {
                if (curr.children[i].str.length() > longestWord.length()) {
                    longestWord = curr.children[i].str;
                }
                dfs(curr.children[i]);
            }
        }
    }
}
```