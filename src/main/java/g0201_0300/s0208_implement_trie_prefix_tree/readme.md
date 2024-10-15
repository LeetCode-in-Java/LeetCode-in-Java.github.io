[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 208\. Implement Trie (Prefix Tree)

Medium

A [**trie**](https://en.wikipedia.org/wiki/Trie) (pronounced as "try") or **prefix tree** is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. There are various applications of this data structure, such as autocomplete and spellchecker.

Implement the Trie class:

*   `Trie()` Initializes the trie object.
*   `void insert(String word)` Inserts the string `word` into the trie.
*   `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
*   `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.

**Example 1:**

**Input**

    ["Trie", "insert", "search", "search", "startsWith", "insert", "search"]
    [[], ["apple"], ["apple"], ["app"], ["app"], ["app"], ["app"]]

**Output:** [null, null, true, false, true, null, true]

**Explanation:**

    Trie trie = new Trie();
    trie.insert("apple"); trie.search("apple"); // return True
    trie.search("app"); // return False
    trie.startsWith("app"); // return True
    trie.insert("app");
    trie.search("app"); // return True 

**Constraints:**

*   `1 <= word.length, prefix.length <= 2000`
*   `word` and `prefix` consist only of lowercase English letters.
*   At most <code>3 * 10<sup>4</sup></code> calls **in total** will be made to `insert`, `search`, and `startsWith`.

## Solution

```java
@SuppressWarnings("java:S1104")
public class Trie {
    private final TrieNode root;
    private boolean startWith;

    private static class TrieNode {
        // Initialize your data structure here.
        public TrieNode[] children;
        public boolean isWord;

        public TrieNode() {
            children = new TrieNode[26];
        }
    }

    public Trie() {
        root = new TrieNode();
    }

    // Inserts a word into the trie.
    public void insert(String word) {
        insert(word, root, 0);
    }

    private void insert(String word, TrieNode root, int idx) {
        if (idx == word.length()) {
            root.isWord = true;
            return;
        }
        int index = word.charAt(idx) - 'a';
        if (root.children[index] == null) {
            root.children[index] = new TrieNode();
        }
        insert(word, root.children[index], idx + 1);
    }

    // Returns if the word is in the trie.
    public boolean search(String word) {
        return search(word, root, 0);
    }

    private boolean search(String word, TrieNode root, int idx) {
        if (idx == word.length()) {
            startWith = true;
            return root.isWord;
        }
        int index = word.charAt(idx) - 'a';
        if (root.children[index] == null) {
            startWith = false;
            return false;
        }
        return search(word, root.children[index], idx + 1);
    }

    // Returns if there is any word in the trie
    // that starts with the given prefix.
    public boolean startsWith(String prefix) {
        search(prefix);
        return startWith;
    }
}

/*
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */
```

**Time Complexity (Big O Time):**

1. **Insertion (insert):**
   - In the `insert` method, the program iterates through the characters of the input word, and for each character, it follows the corresponding child node in the Trie.
   - The number of iterations depends on the length of the word, which is O(word.length()).
   - Since each character is processed once, the insertion of a single word takes O(word.length()) time.

2. **Search (search):**
   - In the `search` method, the program iterates through the characters of the input word, following the corresponding child nodes in the Trie.
   - The number of iterations depends on the length of the word, which is O(word.length()).
   - In the worst case, when the Trie contains a large number of words with the same prefix, searching for a word could take O(word.length()) time.

3. **StartsWith (startsWith):**
   - The `startsWith` method calls the `search` method to find whether any word starts with the given prefix.
   - This is similar to the search operation and also takes O(prefix.length()) time.

Overall, the time complexity for insertion, search, and startsWith operations in the Trie is O(word.length()) or O(prefix.length()), depending on the length of the word or prefix being processed.

**Space Complexity (Big O Space):**

1. **TrieNode Array (children):**
   - The Trie is represented using a tree structure where each node (TrieNode) has an array of children (of size 26 for lowercase English letters).
   - In the worst case, where all words are distinct and there are no common prefixes, the Trie would have a node for each character in all words.
   - The space complexity for the TrieNode array is O(N), where N is the total number of characters in all inserted words.

2. **Recursive Call Stack:**
   - During insertion and search operations, the program uses recursion, which results in a function call stack.
   - The depth of the call stack is bounded by the length of the word or prefix being processed.
   - In the worst case, this depth can be O(word.length()) or O(prefix.length()).
  
3. **Other Variables:**
   - The `root` variable is a constant space requirement.
   - The `startWith` variable is also constant space.

The dominant factor in the space complexity is typically the TrieNode array, which is O(N), where N is the total number of characters in all inserted words.

In summary, the space complexity of this Trie implementation is O(N), and the time complexity for insertion, search, and startsWith operations is O(word.length()) or O(prefix.length()), depending on the length of the word or prefix being processed.
                                                                                                                                   8