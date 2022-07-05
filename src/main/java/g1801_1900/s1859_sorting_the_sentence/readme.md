[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1859\. Sorting the Sentence

Easy

A **sentence** is a list of words that are separated by a single space with no leading or trailing spaces. Each word consists of lowercase and uppercase English letters.

A sentence can be **shuffled** by appending the **1-indexed word position** to each word then rearranging the words in the sentence.

*   For example, the sentence `"This is a sentence"` can be shuffled as `"sentence4 a3 is2 This1"` or `"is2 sentence4 This1 a3"`.

Given a **shuffled sentence** `s` containing no more than `9` words, reconstruct and return _the original sentence_.

**Example 1:**

**Input:** s = "is2 sentence4 This1 a3"

**Output:** "This is a sentence"

**Explanation:** Sort the words in s to their original positions "This1 is2 a3 sentence4", then remove the numbers.

**Example 2:**

**Input:** s = "Myself2 Me1 I4 and3"

**Output:** "Me Myself and I"

**Explanation:** Sort the words in s to their original positions "Me1 Myself2 and3 I4", then remove the numbers.

**Constraints:**

*   `2 <= s.length <= 200`
*   `s` consists of lowercase and uppercase English letters, spaces, and digits from `1` to `9`.
*   The number of words in `s` is between `1` and `9`.
*   The words in `s` are separated by a single space.
*   `s` contains no leading or trailing spaces.

## Solution

```java
import java.util.Map;
import java.util.TreeMap;

public class Solution {
    public String sortSentence(String s) {
        String[] words = s.split(" ");
        TreeMap<Integer, String> treeMap = new TreeMap<>();
        for (String word : words) {
            int key = Integer.parseInt(word.charAt(word.length() - 1) + "");
            treeMap.put(key, word.substring(0, word.length() - 1));
        }
        StringBuilder sb = new StringBuilder();
        for (Map.Entry<Integer, String> entry : treeMap.entrySet()) {
            sb.append(entry.getValue());
            sb.append(" ");
        }
        return sb.substring(0, sb.length() - 1);
    }
}
```