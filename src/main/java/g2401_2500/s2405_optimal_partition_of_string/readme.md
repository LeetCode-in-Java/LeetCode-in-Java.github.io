[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2405\. Optimal Partition of String

Medium

Given a string `s`, partition the string into one or more **substrings** such that the characters in each substring are **unique**. That is, no letter appears in a single substring more than **once**.

Return _the **minimum** number of substrings in such a partition._

Note that each character should belong to exactly one substring in a partition.

**Example 1:**

**Input:** s = "abacaba"

**Output:** 4

**Explanation:**

Two possible partitions are ("a","ba","cab","a") and ("ab","a","ca","ba").

It can be shown that 4 is the minimum number of substrings needed. 

**Example 2:**

**Input:** s = "ssssss"

**Output:** 6

**Explanation:**

The only valid partition is ("s","s","s","s","s","s"). 

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only English lowercase letters.

## Solution

```java
public class Solution {
    public int partitionString(String s) {
        int count = 1;
        boolean[] arr = new boolean[26];
        for (char c : s.toCharArray()) {
            if (arr[c - 'a']) {
                count++;
                arr = new boolean[26];
            }
            arr[c - 'a'] = true;
        }
        return count;
    }
}
```