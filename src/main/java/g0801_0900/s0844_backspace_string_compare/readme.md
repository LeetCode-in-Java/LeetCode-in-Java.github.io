[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 844\. Backspace String Compare

Easy

Given two strings `s` and `t`, return `true` _if they are equal when both are typed into empty text editors_. `'#'` means a backspace character.

Note that after backspacing an empty text, the text will continue empty.

**Example 1:**

**Input:** s = "ab#c", t = "ad#c"

**Output:** true

**Explanation:** Both s and t become "ac".

**Example 2:**

**Input:** s = "ab##", t = "c#d#"

**Output:** true

**Explanation:** Both s and t become "".

**Example 3:**

**Input:** s = "a#c", t = "b"

**Output:** false

**Explanation:** s becomes "c" while t becomes "b".

**Constraints:**

*   `1 <= s.length, t.length <= 200`
*   `s` and `t` only contain lowercase letters and `'#'` characters.

**Follow up:** Can you solve it in `O(n)` time and `O(1)` space?

## Solution

```java
public class Solution {
    public boolean backspaceCompare(String s, String t) {
        return cmprStr(s).equals(cmprStr(t));
    }

    private String cmprStr(String str) {
        StringBuilder res = new StringBuilder();
        int count = 0;
        for (int i = str.length() - 1; i >= 0; i--) {
            char currChar = str.charAt(i);
            if (currChar == '#') {
                count++;
            } else {
                if (count > 0) {
                    count--;
                } else {
                    res.append(currChar);
                }
            }
        }
        return res.toString();
    }
}
```