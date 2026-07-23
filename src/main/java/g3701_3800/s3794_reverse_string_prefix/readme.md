[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3794\. Reverse String Prefix

Easy

You are given a string `s` and an integer `k`.

Reverse the first `k` characters of `s` and return the resulting string.

**Example 1:**

**Input:** s = "abcd", k = 2

**Output:** "bacd"

**Explanation:**

The first `k = 2` characters `"ab"` are reversed to `"ba"`. The final resulting string is `"bacd"`.

**Example 2:**

**Input:** s = "xyz", k = 3

**Output:** "zyx"

**Explanation:**

The first `k = 3` characters `"xyz"` are reversed to `"zyx"`. The final resulting string is `"zyx"`.

**Example 3:**

**Input:** s = "hey", k = 1

**Output:** "hey"

**Explanation:**

The first `k = 1` character `"h"` remains unchanged on reversal. The final resulting string is `"hey"`.

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of lowercase English letters.
*   `1 <= k <= s.length`

## Solution

```java
public class Solution {
    public String reversePrefix(String s, int k) {
        char[] arr = s.toCharArray();
        for (int i = 0; i < k / 2; i++) {
            char temp = arr[i];
            arr[i] = arr[k - 1 - i];
            arr[k - 1 - i] = temp;
        }
        return new String(arr);
    }
}
```