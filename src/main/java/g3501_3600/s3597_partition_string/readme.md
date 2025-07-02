[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3597\. Partition String

Medium

Given a string `s`, partition it into **unique segments** according to the following procedure:

*   Start building a segment beginning at index 0.
*   Continue extending the current segment character by character until the current segment has not been seen before.
*   Once the segment is unique, add it to your list of segments, mark it as seen, and begin a new segment from the next index.
*   Repeat until you reach the end of `s`.

Return an array of strings `segments`, where `segments[i]` is the <code>i<sup>th</sup></code> segment created.

**Example 1:**

**Input:** s = "abbccccd"

**Output:** ["a","b","bc","c","cc","d"]

**Explanation:**

Here is your table, converted from HTML to Markdown:

| Index | Segment After Adding | Seen Segments         | Current Segment Seen Before? | New Segment | Updated Seen Segments            |
|-------|----------------------|-----------------------|------------------------------|-------------|----------------------------------|
| 0     | "a"                  | []                    | No                           | ""          | ["a"]                            |
| 1     | "b"                  | ["a"]                 | No                           | ""          | ["a", "b"]                       |
| 2     | "b"                  | ["a", "b"]            | Yes                          | "b"         | ["a", "b"]                       |
| 3     | "bc"                 | ["a", "b"]            | No                           | ""          | ["a", "b", "bc"]                 |
| 4     | "c"                  | ["a", "b", "bc"]      | No                           | ""          | ["a", "b", "bc", "c"]            |
| 5     | "c"                  | ["a", "b", "bc", "c"] | Yes                          | "c"         | ["a", "b", "bc", "c"]            |
| 6     | "cc"                 | ["a", "b", "bc", "c"] | No                           | ""          | ["a", "b", "bc", "c", "cc"]      |
| 7     | "d"                  | ["a", "b", "bc", "c", "cc"] | No                     | ""          | ["a", "b", "bc", "c", "cc", "d"] |

Hence, the final output is `["a", "b", "bc", "c", "cc", "d"]`.

**Example 2:**

**Input:** s = "aaaa"

**Output:** ["a","aa"]

**Explanation:**

Here is your table converted to Markdown:

| Index | Segment After Adding | Seen Segments | Current Segment Seen Before? | New Segment | Updated Seen Segments |
|-------|----------------------|---------------|------------------------------|-------------|----------------------|
| 0     | "a"                  | []            | No                           | ""          | ["a"]                |
| 1     | "a"                  | ["a"]         | Yes                          | "a"         | ["a"]                |
| 2     | "aa"                 | ["a"]         | No                           | ""          | ["a", "aa"]          |
| 3     | "a"                  | ["a", "aa"]   | Yes                          | "a"         | ["a", "aa"]          |

Hence, the final output is `["a", "aa"]`.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` contains only lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private static class Trie {
        Trie[] tries = new Trie[26];
    }

    public List<String> partitionString(String s) {
        Trie trie = new Trie();
        List<String> res = new ArrayList<>();
        Trie node = trie;
        int i = 0;
        int j = 0;
        while (i < s.length() && j < s.length()) {
            int idx = s.charAt(j) - 'a';
            if (node.tries[idx] == null) {
                res.add(s.substring(i, j + 1));
                node.tries[idx] = new Trie();
                i = j + 1;
                j = i;
                node = trie;
            } else {
                node = node.tries[idx];
                j++;
            }
        }
        return res;
    }
}
```