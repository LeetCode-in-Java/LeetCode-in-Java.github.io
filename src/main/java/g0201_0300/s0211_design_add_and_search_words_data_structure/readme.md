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

*   `1 <= word.length <= 500`
*   `word` in `addWord` consists lower-case English letters.
*   `word` in `search` consist of `'.'` or lower-case English letters.
*   At most `50000` calls will be made to `addWord` and `search`.

## Solution

```java
public class WordDictionary {

    private static class Node {
        char value;
        boolean isEnd;
        Node[] childs;

        Node(char value) {
            this.value = value;
            this.isEnd = false;
            this.childs = new Node[26];
        }

        Node getChild(char ch) {
            return this.childs[ch - 'a'];
        }

        boolean isChild(char ch) {
            return getChild(ch) != null;
        }

        void addChild(char ch) {
            this.childs[ch - 'a'] = new Node(ch);
        }
    }
    // dummy value
    private Node root = new Node('a');

    public WordDictionary() {
        // empty constructor
    }

    public void addWord(String word) {
        Node node = root;
        for (int i = 0; i < word.length(); i++) {
            char ch = word.charAt(i);
            if (!node.isChild(ch)) {
                node.addChild(ch);
            }
            node = node.getChild(ch);
        }
        node.isEnd = true;
    }

    public boolean search(String word) {
        return dfs(this.root, word, 0);
    }

    public boolean dfs(Node root, String word, int index) {
        if (root == null) {
            return false;
        }
        // if reached end of word
        if (index == word.length()) {
            return root.isEnd;
        }
        char ch = word.charAt(index);
        if (ch == '.') {
            for (Node child : root.childs) {
                boolean found = dfs(child, word, index + 1);
                if (found) {
                    return true;
                }
            }
            return false;
        }
        if (!root.isChild(ch)) {
            return false;
        }
        return dfs(root.getChild(ch), word, index + 1);
    }
}

/*
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */
```