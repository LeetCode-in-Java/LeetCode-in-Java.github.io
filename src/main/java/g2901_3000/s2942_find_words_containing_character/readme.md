[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2942\. Find Words Containing Character

Easy

You are given a **0-indexed** array of strings `words` and a character `x`.

Return _an **array of indices** representing the words that contain the character_ `x`.

**Note** that the returned array may be in **any** order.

**Example 1:**

**Input:** words = ["leet","code"], x = "e"

**Output:** [0,1]

**Explanation:** "e" occurs in both words: "l**<ins>ee</ins>**t", and "cod<ins>**e**</ins>". Hence, we return indices 0 and 1. 

**Example 2:**

**Input:** words = ["abc","bcd","aaaa","cbc"], x = "a"

**Output:** [0,2]

**Explanation:** "a" occurs in "**<ins>a</ins>**bc", and "<ins>**aaaa**</ins>". Hence, we return indices 0 and 2. 

**Example 3:**

**Input:** words = ["abc","bcd","aaaa","cbc"], x = "z"

**Output:** []

**Explanation:** "z" does not occur in any of the words. Hence, we return an empty array. 

**Constraints:**

*   `1 <= words.length <= 50`
*   `1 <= words[i].length <= 50`
*   `x` is a lowercase English letter.
*   `words[i]` consists only of lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> findWordsContaining(String[] words, char x) {
        List<Integer> ans = new ArrayList<>();
        for (int i = 0; i < words.length; i++) {
            for (int j = 0; j < words[i].length(); j++) {
                if (words[i].charAt(j) == x) {
                    ans.add(i);
                    break;
                }
            }
        }
        return ans;
    }
}
```