[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3557\. Find Maximum Number of Non Intersecting Substrings

Medium

You are given a string `word`.

Return the **maximum** number of non-intersecting ****substring**** of word that are at **least** four characters long and start and end with the same letter.

**Example 1:**

**Input:** word = "abcdeafdef"

**Output:** 2

**Explanation:**

The two substrings are `"abcdea"` and `"fdef"`.

**Example 2:**

**Input:** word = "bcdaaaab"

**Output:** 1

**Explanation:**

The only substring is `"aaaa"`. Note that we cannot **also** choose `"bcdaaaab"` since it intersects with the other substring.

**Constraints:**

*   <code>1 <= word.length <= 2 * 10<sup>5</sup></code>
*   `word` consists only of lowercase English letters.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int maxSubstrings(String s) {
        int[] prev = new int[26];
        int r = 0;
        Arrays.fill(prev, -1);
        for (int i = 0; i < s.length(); ++i) {
            int j = s.charAt(i) - 'a';
            if (prev[j] != -1 && i - prev[j] + 1 >= 4) {
                ++r;
                Arrays.fill(prev, -1);
            } else if (prev[j] == -1) {
                prev[j] = i;
            }
        }
        return r;
    }
}
```