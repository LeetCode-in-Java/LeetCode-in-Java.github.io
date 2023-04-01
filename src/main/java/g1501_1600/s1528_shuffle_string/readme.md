[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1528\. Shuffle String

Easy

You are given a string `s` and an integer array `indices` of the **same length**. The string `s` will be shuffled such that the character at the <code>i<sup>th</sup></code> position moves to `indices[i]` in the shuffled string.

Return _the shuffled string_.

**Example 1:**

![](https://assets.leetcode.com/uploads/2020/07/09/q1.jpg)

**Input:** s = "codeleet", `indices` = [4,5,6,7,0,2,1,3]

**Output:** "leetcode"

**Explanation:** As shown, "codeleet" becomes "leetcode" after shuffling.

**Example 2:**

**Input:** s = "abc", `indices` = [0,1,2]

**Output:** "abc"

**Explanation:** After shuffling, each character remains in its position.

**Constraints:**

*   `s.length == indices.length == n`
*   `1 <= n <= 100`
*   `s` consists of only lowercase English letters.
*   `0 <= indices[i] < n`
*   All values of `indices` are **unique**.

## Solution

```java
public class Solution {
    public String restoreString(String s, int[] indices) {
        char[] c = new char[s.length()];
        for (int i = 0; i < s.length(); i++) {
            int index = findIndex(indices, i);
            c[i] = s.charAt(index);
        }
        return new String(c);
    }

    private static int findIndex(int[] indices, int i) {
        for (int j = 0; j < indices.length; j++) {
            if (indices[j] == i) {
                return j;
            }
        }
        return 0;
    }
}
```