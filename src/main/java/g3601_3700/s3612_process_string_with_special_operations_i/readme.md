[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3612\. Process String with Special Operations I

Medium

You are given a string `s` consisting of lowercase English letters and the special characters: `*`, `#`, and `%`.

Build a new string `result` by processing `s` according to the following rules from left to right:

*   If the letter is a **lowercase** English letter append it to `result`.
*   A `'*'` **removes** the last character from `result`, if it exists.
*   A `'#'` **duplicates** the current `result` and **appends** it to itself.
*   A `'%'` **reverses** the current `result`.

Return the final string `result` after processing all characters in `s`.

**Example 1:**

**Input:** s = "a#b%\*"

**Output:** "ba"

**Explanation:**

`i`

`s[i]`

Operation

Current `result`

0

`'a'`

Append `'a'`

`"a"`

1

`'#'`

Duplicate `result`

`"aa"`

2

`'b'`

Append `'b'`

`"aab"`

3

`'%'`

Reverse `result`

`"baa"`

4

`'*'`

Remove the last character

`"ba"`

Thus, the final `result` is `"ba"`.

**Example 2:**

**Input:** s = "z\*#"

**Output:** ""

**Explanation:**

`i`

`s[i]`

Operation

Current `result`

0

`'z'`

Append `'z'`

`"z"`

1

`'*'`

Remove the last character

`""`

2

`'#'`

Duplicate the string

`""`

Thus, the final `result` is `""`.

**Constraints:**

*   `1 <= s.length <= 20`
*   `s` consists of only lowercase English letters and special characters `*`, `#`, and `%`.

## Solution

```java
public class Solution {
    public String processStr(String s) {
        StringBuilder res = new StringBuilder();
        for (char c : s.toCharArray()) {
            if (c != '*' && c != '#' && c != '%') {
                res.append(c);
            } else if (c == '#') {
                res.append(res);
            } else if (c == '%') {
                res.reverse();
            } else {
                if (!res.isEmpty()) {
                    res.deleteCharAt(res.length() - 1);
                }
            }
        }
        return res.toString();
    }
}
```