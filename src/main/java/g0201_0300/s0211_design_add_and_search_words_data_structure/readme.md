[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 211\. Design Add and Search Words Data Structure

Medium

Design a data structure that supports adding new words and finding if a string matches any previously added string.

Implement the `WordDictionary` class:

*   `WordDictionary()` Initializes the object.
*   `void addWord(word)` Adds `word` to the data structure, it can be matched later.
*   `bool search(word)` Returns `true` if there is any string in the data structure that matches `word` or `false` otherwise. `word` may contain dots `'.'` where dots can be matched with any letter.

**Example:**

**Input**

    ["WordDictionary","addWord","addWord","addWord","search","search","search","search"] [[],["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]
    
**Output**

    [null,null,null,null,false,true,true,true]

**Explanation**

    WordDictionary wordDictionary = new WordDictionary();
    wordDictionary.addWord("bad");
    wordDictionary.addWord("dad");
    wordDictionary.addWord("mad");
    wordDictionary.search("pad"); // return False
    wordDictionary.search("bad"); // return True
    wordDictionary.search(".ad"); // return True
    wordDictionary.search("b.."); // return True 

**Constraints:**

*   `1 <= word.length <= 25`
*   `word` in `addWord` consists of lowercase English letters.
*   `word` in `search` consist of `'.'` or lowercase English letters.
*   There will be at most `2` dots in `word` for `search` queries.
*   At most <code>10<sup>4</sup></code> calls will be made to `addWord` and `search`.

## Solution

```java
public class WordDictionary {

    public WordDictionary() {
        // empty constructor
    }

    private static class Node {
        Node[] kids = new Node[26];
        boolean isTerminal;
    }

    private final Node[] root = new Node[26];

    public void addWord(String word) {
        int n = word.length();
        if (root[n] == null) {
            root[n] = new Node();
        }
        Node node = root[n];
        for (int i = 0; i < n; i++) {
            int c = word.charAt(i) - 'a';
            Node kid = node.kids[c];
            if (kid == null) {
                kid = new Node();
                node.kids[c] = kid;
            }
            node = kid;
        }
        node.isTerminal = true;
    }

    public boolean search(String word) {
        Node node = root[word.length()];
        return node != null && dfs(0, node, word);
    }

    private boolean dfs(int i, Node node, String word) {
        int len = word.length();
        if (i == len) {
            return false;
        }
        char c = word.charAt(i);
        if (c == '.') {
            for (Node kid : node.kids) {
                if (kid == null) {
                    continue;
                }
                if (i == len - 1 && kid.isTerminal || dfs(i + 1, kid, word)) {
                    return true;
                }
            }
            return false;
        }
        Node kid = node.kids[c - 'a'];
        if (kid == null) {
            return false;
        }
        if (i == len - 1) {
            return kid.isTerminal;
        }
        return dfs(i + 1, kid, word);
    }
}

/*
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```