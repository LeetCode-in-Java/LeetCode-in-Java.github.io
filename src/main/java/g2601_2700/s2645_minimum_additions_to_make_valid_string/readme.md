[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2645\. Minimum Additions to Make Valid String

Medium

Given a string `word` to which you can insert letters "a", "b" or "c" anywhere and any number of times, return _the minimum number of letters that must be inserted so that `word` becomes **valid**._

A string is called **valid** if it can be formed by concatenating the string "abc" several times.

**Example 1:**

**Input:** word = "b"

**Output:** 2

**Explanation:** Insert the letter "a" right before "b", and the letter "c" right next to "a" to obtain the valid string "**a**b**c**".

**Example 2:**

**Input:** word = "aaa"

**Output:** 6

**Explanation:** Insert letters "b" and "c" next to each "a" to obtain the valid string "a**bc**a**bc**a**bc**".

**Example 3:**

**Input:** word = "abc"

**Output:** 0

**Explanation:** word is already valid. No modifications are needed.

**Constraints:**

*   `1 <= word.length <= 50`
*   `word` consists of letters "a", "b" and "c" only.

## Solution

```java
public class Solution {
    public int addMinimum(String word) {
        int res = 0;
        char last = word.charAt(0);
        res += word.charAt(0) - 'a';
        for (int i = 1; i < word.length(); i++) {
            char curr = word.charAt(i);
            if (curr > last) {
                res += curr - last - 1;
            } else {
                res += curr - last + 2;
            }
            last = curr;
        }
        res += 'c' - last;
        return res;
    }
}
```