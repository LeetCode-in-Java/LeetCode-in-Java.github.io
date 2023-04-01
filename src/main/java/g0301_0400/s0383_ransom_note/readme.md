[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 383\. Ransom Note

Easy

Given two stings `ransomNote` and `magazine`, return `true` if `ransomNote` can be constructed from `magazine` and `false` otherwise.

Each letter in `magazine` can only be used once in `ransomNote`.

**Example 1:**

**Input:** ransomNote = "a", magazine = "b"

**Output:** false

**Example 2:**

**Input:** ransomNote = "aa", magazine = "ab"

**Output:** false

**Example 3:**

**Input:** ransomNote = "aa", magazine = "aab"

**Output:** true

**Constraints:**

*   <code>1 <= ransomNote.length, magazine.length <= 10<sup>5</sup></code>
*   `ransomNote` and `magazine` consist of lowercase English letters.

## Solution

```java
public class Solution {
    public boolean canConstruct(String ransomNote, String magazine) {
        int[] a = new int[26];
        int n = ransomNote.length();
        for (int i = 0; i < n; i++) {
            a[ransomNote.charAt(i) - 97]++;
        }
        for (int i = 0; i < magazine.length() && n != 0; i++) {
            if (a[magazine.charAt(i) - 97] > 0) {
                n--;
                a[magazine.charAt(i) - 97]--;
            }
        }
        return n == 0;
    }
}
```