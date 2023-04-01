[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1209\. Remove All Adjacent Duplicates in String II

Medium

You are given a string `s` and an integer `k`, a `k` **duplicate removal** consists of choosing `k` adjacent and equal letters from `s` and removing them, causing the left and the right side of the deleted substring to concatenate together.

We repeatedly make `k` **duplicate removals** on `s` until we no longer can.

Return the final string after all such duplicate removals have been made. It is guaranteed that the answer is unique.

**Example 1:**

**Input:** s = "abcd", k = 2

**Output:** "abcd"

**Explanation:** There's nothing to delete.

**Example 2:**

**Input:** s = "deeedbbcccbdaa", k = 3

**Output:** "aa"

**Explanation:** 

First delete "eee" and "ccc", get "ddbbbdaa" 

Then delete "bbb", get "dddaa" 

Finally delete "ddd", get "aa"

**Example 3:**

**Input:** s = "pbbcggttciiippooaais", k = 2

**Output:** "ps"

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   <code>2 <= k <= 10<sup>4</sup></code>
*   `s` only contains lower case English letters.

## Solution

```java
public class Solution {
    public String removeDuplicates(String s, int k) {
        StringBuilder sb = new StringBuilder();
        int dupCount = 0;
        for (int i = 0; i < s.length(); i++) {
            if (sb.length() != 0 && sb.charAt(sb.length() - 1) == s.charAt(i)) {
                dupCount++;
            } else {
                dupCount = 1;
            }
            sb.append(s.charAt(i));
            if (dupCount == k) {
                sb.setLength(sb.length() - k);
                if (i + 1 < s.length()) {
                    dupCount = 0;
                    for (int j = sb.length() - 1; j >= 0; j--) {
                        if (sb.charAt(j) == s.charAt(i + 1)) {
                            dupCount++;
                        } else {
                            break;
                        }
                    }
                }
            }
        }
        return sb.toString();
    }
}
```