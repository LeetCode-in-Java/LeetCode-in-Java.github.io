[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1941\. Check if All Characters Have Equal Number of Occurrences

Easy

Given a string `s`, return `true` _if_ `s` _is a **good** string, or_ `false` _otherwise_.

A string `s` is **good** if **all** the characters that appear in `s` have the **same** number of occurrences (i.e., the same frequency).

**Example 1:**

**Input:** s = "abacbc"

**Output:** true

**Explanation:** The characters that appear in s are 'a', 'b', and 'c'. All characters occur 2 times in s.

**Example 2:**

**Input:** s = "aaabb"

**Output:** false

**Explanation:** The characters that appear in s are 'a' and 'b'. 'a' occurs 3 times while 'b' occurs 2 times, which is not the same number of times.

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consists of lowercase English letters.

## Solution

```java
public class Solution {
    public boolean areOccurrencesEqual(String s) {
        int[] counter = new int[26];
        for (int i = 0; i < s.length(); i++) {
            counter[s.charAt(i) - 'a']++;
        }
        int bench = 0;
        for (int i = 0; i < 26; i++) {
            if (bench == 0) {
                if (counter[i] != 0) {
                    bench = counter[i];
                }
            } else {
                if (counter[i] != 0 && counter[i] != bench) {
                    return false;
                }
            }
        }
        return true;
    }
}
```