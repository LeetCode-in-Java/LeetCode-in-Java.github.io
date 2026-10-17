[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3926\. Count Valid Word Occurrences

Medium

You are given an array of strings `chunks`. Concatenate all strings in `chunks` in order to form a string `s`.

You are also given an array of strings `queries`.

A **joiner hyphen** is a hyphen character `'-'` in `s` whose previous and next characters both exist and are lowercase English letters.

A **word** is a **maximal** substring of `s` consisting only of lowercase English letters and **joiner hyphens**.

All other characters, including spaces and hyphens that are not **joiner hyphens**, are treated as separators.

Return an integer array `ans`, where `ans[i]` is the number of times `queries[i]` appears as a word in `s`.

**Example 1:**

**Input:** chunks = ["hello wor","ld hello"], queries = ["hello","world","wor"]

**Output:** [2,1,0]

**Explanation:**

*   After concatenating all strings in `chunks`, `s = "hello world hello"`.
*   The words are `"hello"`, `"world"`, and `"hello"`.
*   The substring `"wor"` appears inside `"world"`, but it is not a full word.

**Example 2:**

**Input:** chunks = ["a-b a--b ","a-","b"], queries = ["a-b","a","b"]

**Output:** [2,1,1]

**Explanation:**

*   After concatenating all strings in `chunks`, `s = "a-b a--b a-b"`.
*   In `"a-b"`, the hyphen is a joiner hyphen because it is between two lowercase English letters, so `"a-b"` is one word.
*   In `"a--b"`, neither hyphen is a joiner hyphen, so it is split into the words `"a"` and `"b"`.
*   Therefore, the words are `"a-b"`, `"a"`, `"b"`, and `"a-b"`.

**Example 3:**

**Input:** chunks = ["-cat dog- mouse"], queries = ["cat","dog","mouse","cat-dog"]

**Output:** [1,1,1,0]

**Explanation:**

*   After concatenating all strings in `chunks`, `s = "-cat dog- mouse"`.
*   The leading hyphen before `"cat"` and the trailing hyphen after `"dog"` are not joiner hyphens, so they are separators.
*   The words are `"cat"`, `"dog"`, and `"mouse"`.

**Constraints:**

*   <code>1 <= chunks.length <= 10<sup>5</sup></code>
*   <code>1 <= chunks[i].length <= 10<sup>5</sup></code>
*   The total length of all strings in `chunks` does not exceed <code>10<sup>5</sup></code>.
*   `chunks[i]` consists only of lowercase English letters, spaces, and `'-'`.
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   <code>1 <= queries[i].length <= 10<sup>5</sup></code>
*   The total length of all strings in `queries` does not exceed <code>10<sup>5</sup></code>.
*   `queries[i]` consists only of lowercase English letters and `'-'`.
*   `queries[i]` is a valid word: it does not start or end with `'-'`, and it does not contain two consecutive hyphens.

## Solution

```java
public class Solution {
    public int[] countWordOccurrences(String[] chunks, String[] queries) {
        Trie trie = new Trie();
        trie.fill(chunks);
        int n = queries.length;
        int[] result = new int[n];
        for (int k = 0; k < n; k++) {
            result[k] = trie.count(queries[k]);
        }
        return result;
    }

    private static class Trie {
        private final Trie[] children = new Trie[27];
        private int count = 0;

        private void fill(String[] chunks) {
            Trie node = this;
            char last = '@';
            for (int j = 0; j < chunks.length; j++) {
                String chunk = chunks[j];
                for (int k = 0; k < chunk.length(); k++) {
                    char c = processCharacter(chunk, k, j, chunks, last);
                    node = addCharacter(node, c);
                    node = finalizeWordIfNeeded(node, c);
                    last = c;
                }
            }
            incrementCountIfNeeded(node);
        }

        private char processCharacter(
                String chunk, int index, int chunkIndex, String[] chunks, char last) {
            char c = chunk.charAt(index);
            if (c == '-') {
                char next = getNextCharacter(chunk, index, chunkIndex, chunks);
                if (!isLetter(last) || !isLetter(next)) {
                    return '@';
                }
            }
            return c;
        }

        private char getNextCharacter(String chunk, int index, int chunkIndex, String[] chunks) {
            if (index < chunk.length() - 1) {
                return chunk.charAt(index + 1);
            }
            if (chunkIndex < chunks.length - 1) {
                return chunks[chunkIndex + 1].charAt(0);
            }
            return '@';
        }

        private Trie addCharacter(Trie node, char c) {
            if (!isValidCharacter(c)) {
                return node;
            }
            int index = getIndex(c);
            if (node.children[index] == null) {
                node.children[index] = new Trie();
            }
            return node.children[index];
        }

        private Trie finalizeWordIfNeeded(Trie node, char c) {
            if (isValidCharacter(c)) {
                return node;
            }
            incrementCountIfNeeded(node);
            return this;
        }

        private void incrementCountIfNeeded(Trie node) {
            if (node != this) {
                node.count++;
            }
        }

        private static boolean isValidCharacter(char c) {
            return c == '-' || isLetter(c);
        }

        private int count(String s) {
            int n = s.length();
            Trie node = this;
            for (int k = 0; k < n; k++) {
                int i = getIndex(s.charAt(k));
                if (node.children[i] == null) {
                    return 0;
                }
                node = node.children[i];
            }
            return node.count;
        }

        private static int getIndex(char c) {
            return c == '-' ? 26 : c - 'a';
        }

        private static boolean isLetter(char c) {
            return 'a' <= c && c <= 'z';
        }
    }
}
```