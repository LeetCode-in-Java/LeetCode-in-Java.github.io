## 648\. Replace Words

Medium

In English, we have a concept called **root**, which can be followed by some other word to form another longer word - let's call this word **successor**. For example, when the **root** `"an"` is followed by the **successor** word `"other"`, we can form a new word `"another"`.

Given a `dictionary` consisting of many **roots** and a `sentence` consisting of words separated by spaces, replace all the **successors** in the sentence with the **root** forming it. If a **successor** can be replaced by more than one **root**, replace it with the **root** that has **the shortest length**.

Return _the `sentence`_ after the replacement.

**Example 1:**

**Input:** dictionary = ["cat","bat","rat"], sentence = "the cattle was rattled by the battery"

**Output:** "the cat was rat by the bat"

**Example 2:**

**Input:** dictionary = ["a","b","c"], sentence = "aadsfasf absbs bbab cadsfafs"

**Output:** "a a b c"

**Constraints:**

*   `1 <= dictionary.length <= 1000`
*   `1 <= dictionary[i].length <= 100`
*   `dictionary[i]` consists of only lower-case letters.
*   <code>1 <= sentence.length <= 10<sup>6</sup></code>
*   `sentence` consists of only lower-case letters and spaces.
*   The number of words in `sentence` is in the range `[1, 1000]`
*   The length of each word in `sentence` is in the range `[1, 1000]`
*   Every two consecutive words in `sentence` will be separated by exactly one space.
*   `sentence` does not have leading or trailing spaces.

## Solution

```java
import java.util.List;

public class Solution {
    public String replaceWords(List<String> dictionary, String sentence) {
        Trie trie = new Trie();
        dictionary.forEach(trie::insert);
        String[] allWords = sentence.split(" ");
        for (int i = 0; i < allWords.length; i++) {
            allWords[i] = trie.getRootForWord(allWords[i]);
        }
        return String.join(" ", allWords);
    }

    static class Node {
        Node[] links = new Node[26];
        boolean wordCompleted;

        public boolean containsKey(char ch) {
            return links[ch - 'a'] != null;
        }

        public void put(char ch, Node node) {
            links[ch - 'a'] = node;
        }

        public Node get(char ch) {
            return links[ch - 'a'];
        }

        public boolean isWordCompleted() {
            return wordCompleted;
        }

        public void setWordCompleted(boolean flag) {
            wordCompleted = flag;
        }
    }

    static class Trie {
        Node root;

        public Trie() {
            root = new Node();
        }

        public void insert(String word) {
            Node node = root;
            for (int i = 0; i < word.length(); i++) {
                if (!node.containsKey(word.charAt(i))) {
                    node.put(word.charAt(i), new Node());
                }
                node = node.get(word.charAt(i));
            }
            node.setWordCompleted(true);
        }

        public String getRootForWord(String word) {
            Node node = root;
            StringBuilder rootWord = new StringBuilder();
            for (int i = 0; i < word.length(); i++) {
                if (node.containsKey(word.charAt(i))) {
                    rootWord.append(word.charAt(i));
                    node = node.get(word.charAt(i));
                    if (node.isWordCompleted()) {
                        return rootWord.toString();
                    }

                } else {
                    return word;
                }
            }
            return word;
        }
    }
}
```