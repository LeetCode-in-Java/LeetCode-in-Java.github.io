[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2839\. Check if Strings Can be Made Equal With Operations I

Easy

You are given two strings `s1` and `s2`, both of length `4`, consisting of **lowercase** English letters.

You can apply the following operation on any of the two strings **any** number of times:

*   Choose any two indices `i` and `j` such that `j - i = 2`, then **swap** the two characters at those indices in the string.

Return `true` _if you can make the strings_ `s1` _and_ `s2` _equal, and_ `false` _otherwise_.

**Example 1:**

**Input:** s1 = "abcd", s2 = "cdab"

**Output:** true

**Explanation:** We can do the following operations on s1: 
- Choose the indices i = 0, j = 2. The resulting string is s1 = "cbad". 
- Choose the indices i = 1, j = 3. The resulting string is s1 = "cdab" = s2.

**Example 2:**

**Input:** s1 = "abcd", s2 = "dacb"

**Output:** false

**Explanation:** It is not possible to make the two strings equal.

**Constraints:**

*   `s1.length == s2.length == 4`
*   `s1` and `s2` consist only of lowercase English letters.

## Solution

```java
public class Solution {
    public boolean canBeEqual(String s1, String s2) {
        return isOk(s1, s2, 0) && isOk(s1, s2, 1);
    }

    private boolean isOk(String s1, String s2, int i) {
        char a = s1.charAt(i);
        char b = s1.charAt(i + 2);
        char c = s2.charAt(i);
        char d = s2.charAt(i + 2);
        if (a == c && b == d) {
            return true;
        }
        return a == d && b == c;
    }
}
```