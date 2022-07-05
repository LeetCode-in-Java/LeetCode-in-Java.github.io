[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 925\. Long Pressed Name

Easy

Your friend is typing his `name` into a keyboard. Sometimes, when typing a character `c`, the key might get _long pressed_, and the character will be typed 1 or more times.

You examine the `typed` characters of the keyboard. Return `True` if it is possible that it was your friends name, with some characters (possibly none) being long pressed.

**Example 1:**

**Input:** name = "alex", typed = "aaleex"

**Output:** true

**Explanation:** 'a' and 'e' in 'alex' were long pressed.

**Example 2:**

**Input:** name = "saeed", typed = "ssaaedd"

**Output:** false

**Explanation:** 'e' must have been pressed twice, but it was not in the typed output.

**Constraints:**

*   `1 <= name.length, typed.length <= 1000`
*   `name` and `typed` consist of only lowercase English letters.

## Solution

```java
public class Solution {
    public boolean isLongPressedName(String name, String typed) {
        int i = 0;
        int j = 0;
        char prev = '$';
        if (typed.length() < name.length()) {
            return false;
        }
        while (i < name.length() && j < typed.length()) {
            while (j < typed.length() && typed.charAt(j) != name.charAt(i)) {
                if (typed.charAt(j) != prev) {
                    return false;
                }
                if (j == typed.length() - 1) {
                    return false;
                }
                j++;
            }
            prev = name.charAt(i);
            i++;
            j++;
        }
        if (i < name.length()) {
            return false;
        }
        for (; j < typed.length(); j++) {
            if (typed.charAt(j) != prev) {
                return false;
            }
        }
        return true;
    }
}
```