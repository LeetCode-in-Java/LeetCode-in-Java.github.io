[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 820\. Short Encoding of Words

Medium

A **valid encoding** of an array of `words` is any reference string `s` and array of indices `indices` such that:

*   `words.length == indices.length`
*   The reference string `s` ends with the `'#'` character.
*   For each index `indices[i]`, the **substring** of `s` starting from `indices[i]` and up to (but not including) the next `'#'` character is equal to `words[i]`.

Given an array of `words`, return _the **length of the shortest reference string**_ `s` _possible of any **valid encoding** of_ `words`_._

**Example 1:**

**Input:** words = ["time", "me", "bell"]

**Output:** 10

**Explanation:** A valid encoding would be s = `"time#bell#" and indices = [0, 2, 5`]. 

words[0] = "time", the substring of s starting from indices[0] = 0 to the next '#' is underlined in "time#bell#" 

words[1] = "me", the substring of s starting from indices[1] = 2 to the next '#' is underlined in "time#bell#" 

words[2] = "bell", the substring of s starting from indices[2] = 5 to the next '#' is underlined in "time#bell#"

**Example 2:**

**Input:** words = ["t"]

**Output:** 2

**Explanation:** A valid encoding would be s = "t#" and indices = [0].

**Constraints:**

*   `1 <= words.length <= 2000`
*   `1 <= words[i].length <= 7`
*   `words[i]` consists of only lowercase letters.

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static class Node {
        Node[] nodes = new Node[26];
    }

    private boolean insert(Node node, String word) {
        Node current = node;
        int n = word.length();
        boolean flag = false;
        for (int i = n - 1; i >= 0; i--) {
            if (current.nodes[word.charAt(i) - 'a'] == null) {
                current.nodes[word.charAt(i) - 'a'] = new Node();
                flag = true;
            }
            current = current.nodes[word.charAt(i) - 'a'];
        }
        return flag;
    }

    public int minimumLengthEncoding(String[] words) {
        int out = 0;
        Arrays.sort(words, (a, b) -> b.length() - a.length());
        Node node = new Node();
        for (String word : words) {
            if (insert(node, word)) {
                out = out + word.length() + 1;
            }
        }
        return out;
    }
}
```