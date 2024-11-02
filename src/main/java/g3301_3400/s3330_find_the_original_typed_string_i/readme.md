[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3330\. Find the Original Typed String I

Easy

Alice is attempting to type a specific string on her computer. However, she tends to be clumsy and **may** press a key for too long, resulting in a character being typed **multiple** times.

Although Alice tried to focus on her typing, she is aware that she may still have done this **at most** _once_.

You are given a string `word`, which represents the **final** output displayed on Alice's screen.

Return the total number of _possible_ original strings that Alice _might_ have intended to type.

**Example 1:**

**Input:** word = "abbcccc"

**Output:** 5

**Explanation:**

The possible strings are: `"abbcccc"`, `"abbccc"`, `"abbcc"`, `"abbc"`, and `"abcccc"`.

**Example 2:**

**Input:** word = "abcd"

**Output:** 1

**Explanation:**

The only possible string is `"abcd"`.

**Example 3:**

**Input:** word = "aaaa"

**Output:** 4

**Constraints:**

*   `1 <= word.length <= 100`
*   `word` consists only of lowercase English letters.

## Solution

```java
public class Solution {
    public int possibleStringCount(String word) {
        int n = word.length();
        int count = 1;
        char pre = word.charAt(0);
        int temp = 0;
        for (int i = 1; i < n; i++) {
            char ch = word.charAt(i);
            if (ch == pre) {
                temp++;
            } else {
                if (temp >= 1) {
                    count += temp;
                }
                temp = 0;
                pre = ch;
            }
        }
        if (temp >= 1) {
            count += temp;
        }
        return count;
    }
}
```