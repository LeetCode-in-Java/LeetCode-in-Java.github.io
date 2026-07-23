[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3803\. Count Residue Prefixes

Easy

You are given a string `s` consisting only of lowercase English letters.

A **prefix** of `s` is called a **residue** if the number of **distinct characters** in the **prefix** is equal to `len(prefix) % 3`.

Return the count of **residue** prefixes in `s`.

A **prefix** of a string is a **non-empty substring** that starts from the beginning of the string and extends to any point within it.

**Example 1:**

**Input:** s = "abc"

**Output:** 2

**Explanation:**

*   Prefix `"a"` has 1 distinct character and length modulo 3 is 1, so it is a residue.
*   Prefix `"ab"` has 2 distinct characters and length modulo 3 is 2, so it is a residue.
*   Prefix `"abc"` does not satisfy the condition. Thus, the answer is 2.

**Example 2:**

**Input:** s = "dd"

**Output:** 1

**Explanation:**

*   Prefix `"d"` has 1 distinct character and length modulo 3 is 1, so it is a residue.
*   Prefix `"dd"` has 1 distinct character but length modulo 3 is 2, so it is not a residue. Thus, the answer is 1.

**Example 3:**

**Input:** s = "bob"

**Output:** 2

**Explanation:**

*   Prefix `"b"` has 1 distinct character and length modulo 3 is 1, so it is a residue.
*   Prefix `"bo"` has 2 distinct characters and length mod 3 is 2, so it is a residue. Thus, the answer is 2.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` contains only lowercase English letters.

## Solution

```java
public class Solution {
    public int residuePrefixes(String s) {
        int n = s.length();
        int count = 0;
        int p = 0;
        char c1 = s.charAt(p);
        while (p < n && c1 == s.charAt(p)) {
            if (++p % 3 == 1) {
                count++;
            }
        }
        if (p >= n) {
            return count;
        }
        char c2 = s.charAt(p);
        while (p < n && (c1 == s.charAt(p) || c2 == s.charAt(p))) {
            if (++p % 3 == 2) {
                count++;
            }
        }
        return count;
    }
}
```