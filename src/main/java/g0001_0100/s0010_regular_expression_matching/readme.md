[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 10\. Regular Expression Matching

Hard

Given an input string `s` and a pattern `p`, implement regular expression matching with support for `'.'` and `'*'` where:

*   `'.'` Matches any single character.
*   `'*'` Matches zero or more of the preceding element.

The matching should cover the **entire** input string (not partial).

**Example 1:**

**Input:** s = "aa", p = "a"

**Output:** false

**Explanation:** "a" does not match the entire string "aa". 

**Example 2:**

**Input:** s = "aa", p = "a\*"

**Output:** true

**Explanation:** '\*' means zero or more of the preceding element, 'a'. Therefore, by repeating 'a' once, it becomes "aa". 

**Example 3:**

**Input:** s = "ab", p = ".\*"

**Output:** true

**Explanation:** ".\*" means "zero or more (\*) of any character (.)". 

**Example 4:**

**Input:** s = "aab", p = "c\*a\*b"

**Output:** true

**Explanation:** c can be repeated 0 times, a can be repeated 1 time. Therefore, it matches "aab". 

**Example 5:**

**Input:** s = "mississippi", p = "mis\*is\*p\*."

**Output:** false 

**Constraints:**

*   `1 <= s.length <= 20`
*   `1 <= p.length <= 30`
*   `s` contains only lowercase English letters.
*   `p` contains only lowercase English letters, `'.'`, and `'*'`.
*   It is guaranteed for each appearance of the character `'*'`, there will be a previous valid character to match.

## Solution

```java
public class Solution {
    private Boolean[][] cache;

    public boolean isMatch(String s, String p) {
        cache = new Boolean[s.length() + 1][p.length() + 1];
        return isMatch(s, p, 0, 0);
    }

    private boolean isMatch(String s, String p, int i, int j) {
        if (j == p.length()) {
            return i == s.length();
        }
        boolean result;
        if (cache[i][j] != null) {
            return cache[i][j];
        }
        boolean firstMatch = i < s.length() && (s.charAt(i) == p.charAt(j) || p.charAt(j) == '.');
        if ((j + 1) < p.length() && p.charAt(j + 1) == '*') {
            result = (firstMatch && isMatch(s, p, i + 1, j)) || isMatch(s, p, i, j + 2);
        } else {
            result = firstMatch && isMatch(s, p, i + 1, j + 1);
        }
        cache[i][j] = result;
        return result;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(m*n), where "m" represents the length of the string `s`, and "n" represents the length of the string `p`. Here's the breakdown:

1. The `cache` is a 2D Boolean array with dimensions (s.length() + 1) x (p.length() + 1). Therefore, it has O(m*n) space complexity.

2. The program uses dynamic programming and recursion to determine if the given strings `s` and `p` match. The primary function `isMatch` is called with varying indices `i` and `j` within the ranges [0, m] and [0, n], respectively.

3. The `cache` array is used to memoize the results of subproblems to avoid redundant computations. If the result for a specific `(i, j)` pair is already in the cache, it's retrieved in constant time.

4. The recursion, however, explores all possible combinations of `i` and `j` values. In the worst case, it explores all possible pairs within the range [0, m] and [0, n], resulting in a time complexity of O(m*n).

Therefore, the overall time complexity is O(m*n).

**Space Complexity (Big O Space):**

The space complexity of this program is O(m*n), primarily due to the `cache` 2D Boolean array. Additionally, there's a negligible amount of space used for other variables, such as `result`, `firstMatch`, `i`, and `j`, which are all constants and do not depend on the input size.

Hence, the dominant factor in terms of space complexity is the `cache` array, which has O(m*n) space complexity.
