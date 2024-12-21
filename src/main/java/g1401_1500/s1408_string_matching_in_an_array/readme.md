[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1408\. String Matching in an Array

Easy

Given an array of string `words`. Return all strings in `words` which is substring of another word in **any** order.

String `words[i]` is substring of `words[j]`, if can be obtained removing some characters to left and/or right side of `words[j]`.

**Example 1:**

**Input:** words = ["mass","as","hero","superhero"]

**Output:** ["as","hero"]

**Explanation:** "as" is substring of "mass" and "hero" is substring of "superhero". ["hero","as"] is also a valid answer.

**Example 2:**

**Input:** words = ["leetcode","et","code"]

**Output:** ["et","code"]

**Explanation:** "et", "code" are substring of "leetcode".

**Example 3:**

**Input:** words = ["blue","green","bu"]

**Output:** []

**Constraints:**

*   `1 <= words.length <= 100`
*   `1 <= words[i].length <= 30`
*   `words[i]` contains only lowercase English letters.
*   It's **guaranteed** that `words[i]` will be unique.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<String> stringMatching(String[] words) {
        List<String> matchedStrings = new ArrayList<>();
        for (int i = 0; i < words.length; i++) {
            boolean containsSubstring = checkContains(words, i);
            if (containsSubstring) {
                matchedStrings.add(words[i]);
            }
        }
        return matchedStrings;
    }

    private boolean checkContains(String[] words, int index) {
        for (int j = 0; j < words.length; j++) {
            if (index == j) {
                continue;
            }
            if (words[j].contains(words[index])) {
                return true;
            }
        }
        return false;
    }
}
```