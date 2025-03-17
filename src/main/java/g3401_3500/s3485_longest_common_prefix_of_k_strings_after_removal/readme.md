[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3485\. Longest Common Prefix of K Strings After Removal

Hard

You are given an array of strings `words` and an integer `k`.

For each index `i` in the range `[0, words.length - 1]`, find the **length** of the **longest common prefix** among any `k` strings (selected at **distinct indices**) from the remaining array after removing the <code>i<sup>th</sup></code> element.

Return an array `answer`, where `answer[i]` is the answer for <code>i<sup>th</sup></code> element. If removing the <code>i<sup>th</sup></code> element leaves the array with fewer than `k` strings, `answer[i]` is 0.

**Example 1:**

**Input:** words = ["jump","run","run","jump","run"], k = 2

**Output:** [3,4,4,3,4]

**Explanation:**

*   Removing index 0 (`"jump"`):
    *   `words` becomes: `["run", "run", "jump", "run"]`. `"run"` occurs 3 times. Choosing any two gives the longest common prefix `"run"` (length 3).
*   Removing index 1 (`"run"`):
    *   `words` becomes: `["jump", "run", "jump", "run"]`. `"jump"` occurs twice. Choosing these two gives the longest common prefix `"jump"` (length 4).
*   Removing index 2 (`"run"`):
    *   `words` becomes: `["jump", "run", "jump", "run"]`. `"jump"` occurs twice. Choosing these two gives the longest common prefix `"jump"` (length 4).
*   Removing index 3 (`"jump"`):
    *   `words` becomes: `["jump", "run", "run", "run"]`. `"run"` occurs 3 times. Choosing any two gives the longest common prefix `"run"` (length 3).
*   Removing index 4 ("run"):
    *   `words` becomes: `["jump", "run", "run", "jump"]`. `"jump"` occurs twice. Choosing these two gives the longest common prefix `"jump"` (length 4).

**Example 2:**

**Input:** words = ["dog","racer","car"], k = 2

**Output:** [0,0,0]

**Explanation:**

*   Removing any index results in an answer of 0.

**Constraints:**

*   <code>1 <= k <= words.length <= 10<sup>5</sup></code>
*   <code>1 <= words[i].length <= 10<sup>4</sup></code>
*   `words[i]` consists of lowercase English letters.
*   The sum of `words[i].length` is smaller than or equal <code>10<sup>5</sup></code>.

## Solution

```java
public class Solution {
    private static class TrieNode {
        TrieNode[] children = new TrieNode[26];
        int count = 0;
        int bestPrefixLength = -1;
    }

    private TrieNode root;

    public int[] longestCommonPrefix(String[] words, int k) {
        int totalWords = words.length;
        int[] result = new int[totalWords];
        if (totalWords - 1 < k) {
            return result;
        }
        root = new TrieNode();
        for (String word : words) {
            // insert the word with increasing the count by 1 (prefix count)
            updateTrie(word, 1, k);
        }
        for (int i = 0; i < totalWords; i++) {
            // temp deletion of the word with count decreased by 1
            updateTrie(words[i], -1, k);
            result[i] = root.bestPrefixLength;
            // re-insertion of the word
            updateTrie(words[i], 1, k);
        }
        return result;
    }

    private void updateTrie(String word, int count, int k) {
        int wordLength = word.length();
        // used to store the path used during trie travesal to update the count and use the count
        TrieNode[] nodePath = new TrieNode[wordLength + 1];
        int[] depths = new int[wordLength + 1];
        nodePath[0] = root;
        depths[0] = 0;
        // trie insertion
        for (int i = 0; i < wordLength; i++) {
            int letterIndex = word.charAt(i) - 'a';
            if (nodePath[i].children[letterIndex] == null) {
                nodePath[i].children[letterIndex] = new TrieNode();
            }
            nodePath[i + 1] = nodePath[i].children[letterIndex];
            depths[i + 1] = depths[i] + 1;
        }
        // increase the count of each prefix
        for (TrieNode node : nodePath) {
            node.count += count;
        }
        // find the best prefix
        for (int i = nodePath.length - 1; i >= 0; i--) {
            TrieNode currentNode = nodePath[i];
            int candidate = (currentNode.count >= k ? depths[i] : -1);
            for (TrieNode childNode : currentNode.children) {
                if (childNode != null) {
                    candidate = Math.max(candidate, childNode.bestPrefixLength);
                }
            }
            currentNode.bestPrefixLength = candidate;
        }
    }
}
```