[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1003\. Check If Word Is Valid After Substitutions

Medium

Given a string `s`, determine if it is **valid**.

A string `s` is **valid** if, starting with an empty string `t = ""`, you can **transform** `t` **into** `s` after performing the following operation **any number of times**:

*   Insert string `"abc"` into any position in `t`. More formally, `t` becomes <code>t<sub>left</sub> + "abc" + t<sub>right</sub></code>, where <code>t == t<sub>left</sub> + t<sub>right</sub></code>. Note that <code>t<sub>left</sub></code> and <code>t<sub>right</sub></code> may be **empty**.

Return `true` _if_ `s` _is a **valid** string, otherwise, return_ `false`.

**Example 1:**

**Input:** s = "aabcbc"

**Output:** true

**Explanation:** "" -> "abc" -> "aabcbc" Thus, "aabcbc" is valid.

**Example 2:**

**Input:** s = "abcabcababcc"

**Output:** true

**Explanation:** "" -> "abc" -> "abcabc" -> "abcabcabc" -> "abcabcababcc" Thus, "abcabcababcc" is valid.

**Example 3:**

**Input:** s = "abccba"

**Output:** false

**Explanation:** It is impossible to get "abccba" using the operation.

**Constraints:**

*   <code>1 <= s.length <= 2 * 10<sup>4</sup></code>
*   `s` consists of letters `'a'`, `'b'`, and `'c'`

## Solution

```java
import java.util.ArrayDeque;
import java.util.Deque;

public class Solution {
    public boolean isValid(String s) {
        Deque<Character> stack = new ArrayDeque<>();
        for (char c : s.toCharArray()) {
            if (c == 'c') {
                if (stack.isEmpty() || stack.pop() != 'b') {
                    return false;
                }
                if (stack.isEmpty() || stack.pop() != 'a') {
                    return false;
                }
            } else {
                stack.push(c);
            }
        }
        return stack.isEmpty();
    }
}
```