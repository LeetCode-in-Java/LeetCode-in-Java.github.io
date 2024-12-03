[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1286\. Iterator for Combination

Medium

Design the `CombinationIterator` class:

*   `CombinationIterator(string characters, int combinationLength)` Initializes the object with a string `characters` of **sorted distinct** lowercase English letters and a number `combinationLength` as arguments.
*   `next()` Returns the next combination of length `combinationLength` in **lexicographical order**.
*   `hasNext()` Returns `true` if and only if there exists a next combination.

**Example 1:**

**Input** ["CombinationIterator", "next", "hasNext", "next", "hasNext", "next", "hasNext"] [["abc", 2], [], [], [], [], [], []]

**Output:** [null, "ab", true, "ac", true, "bc", false]

**Explanation:** CombinationIterator itr = new CombinationIterator("abc", 2); itr.next(); // return "ab" itr.hasNext(); // return True itr.next(); // return "ac" itr.hasNext(); // return True itr.next(); // return "bc" itr.hasNext(); // return False

**Constraints:**

*   `1 <= combinationLength <= characters.length <= 15`
*   All the characters of `characters` are **unique**.
*   At most <code>10<sup>4</sup></code> calls will be made to `next` and `hasNext`.
*   It is guaranteed that all calls of the function `next` are valid.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class CombinationIterator {
    private List<String> list;
    private int index;
    private int combinationLength;

    public CombinationIterator(String characters, int combinationLength) {
        this.index = 0;
        this.list = new ArrayList<>();
        this.combinationLength = combinationLength;
        boolean[] visited = new boolean[characters.length()];
        buildAllCombinations(characters, 0, new StringBuilder(), visited);
    }

    private void buildAllCombinations(
            String characters, int start, StringBuilder sb, boolean[] visited) {
        if (sb.length() == combinationLength) {
            list.add(sb.toString());
        } else {
            int i = start;
            while (i < characters.length()) {
                if (!visited[i]) {
                    sb.append(characters.charAt(i));
                    visited[i] = true;
                    buildAllCombinations(characters, i++, sb, visited);
                    visited[i - 1] = false;
                    sb.setLength(sb.length() - 1);
                } else {
                    i++;
                }
            }
        }
    }

    public String next() {
        return list.get(index++);
    }

    public boolean hasNext() {
        return index < list.size();
    }
}

/*
 * Your CombinationIterator object will be instantiated and called as such:
 * CombinationIterator obj = new CombinationIterator(characters, combinationLength);
 * String param_1 = obj.next();
 * boolean param_2 = obj.hasNext();
 */
```