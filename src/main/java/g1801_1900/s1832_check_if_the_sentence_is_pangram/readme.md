[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1832\. Check if the Sentence Is Pangram

Easy

A **pangram** is a sentence where every letter of the English alphabet appears at least once.

Given a string `sentence` containing only lowercase English letters, return `true` _if_ `sentence` _is a **pangram**, or_ `false` _otherwise._

**Example 1:**

**Input:** sentence = "thequickbrownfoxjumpsoverthelazydog"

**Output:** true

**Explanation:** sentence contains at least one of every letter of the English alphabet.

**Example 2:**

**Input:** sentence = "leetcode"

**Output:** false

**Constraints:**

*   `1 <= sentence.length <= 1000`
*   `sentence` consists of lowercase English letters.

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public boolean checkIfPangram(String sentence) {
        Set<Character> alphabet = new HashSet<>();
        for (char c : sentence.toCharArray()) {
            alphabet.add(c);
        }
        return alphabet.size() == 26;
    }
}
```