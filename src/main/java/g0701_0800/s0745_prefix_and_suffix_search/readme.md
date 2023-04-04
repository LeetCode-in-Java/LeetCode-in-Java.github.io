[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 745\. Prefix and Suffix Search

Hard

Design a special dictionary with some words that searchs the words in it by a prefix and a suffix.

Implement the `WordFilter` class:

*   `WordFilter(string[] words)` Initializes the object with the `words` in the dictionary.
*   `f(string prefix, string suffix)` Returns _the index of the word in the dictionary,_ which has the prefix `prefix` and the suffix `suffix`. If there is more than one valid index, return **the largest** of them. If there is no such word in the dictionary, return `-1`.

**Example 1:**

**Input** 

["WordFilter", "f"] 

[[["apple"]], ["a", "e"]]

**Output:** [null, 0]

**Explanation:** 

    WordFilter wordFilter = new WordFilter(["apple"]); 
    wordFilter.f("a", "e"); // return 0, because the word at index 0 has prefix = "a" and suffix = 'e".

**Constraints:**

*   `1 <= words.length <= 15000`
*   `1 <= words[i].length <= 10`
*   `1 <= prefix.length, suffix.length <= 10`
*   `words[i]`, `prefix` and `suffix` consist of lower-case English letters only.
*   At most `15000` calls will be made to the function `f`.

## Solution

```java
public class WordFilter {
    private static class TrieNode {
        TrieNode[] children;
        int weight;

        TrieNode() {
            this.children = new TrieNode[27];
            this.weight = 0;
        }
    }

    private TrieNode root = new TrieNode();

    public WordFilter(String[] words) {
        for (int i = 0; i < words.length; i++) {
            String s = words[i];
            for (int j = 0; j < s.length(); j++) {
                insert(s.substring(j) + '{' + s, i);
            }
        }
    }

    public void insert(String wd, int weight) {
        TrieNode node = root;
        for (char ch : wd.toCharArray()) {
            if (node.children[ch - 'a'] == null) {
                node.children[ch - 'a'] = new TrieNode();
            }
            node = node.children[ch - 'a'];
            node.weight = weight;
        }
    }

    public int f(String prefix, String suffix) {
        return startsWith(suffix + '{' + prefix);
    }

    public int startsWith(String pref) {
        TrieNode node = root;
        for (char ch : pref.toCharArray()) {
            if (node.children[ch - 'a'] == null) {
                return -1;
            }
            node = node.children[ch - 'a'];
        }
        return node.weight;
    }
}
```