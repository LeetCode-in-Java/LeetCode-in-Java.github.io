[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3412\. Find Mirror Score of a String

Medium

You are given a string `s`.

We define the **mirror** of a letter in the English alphabet as its corresponding letter when the alphabet is reversed. For example, the mirror of `'a'` is `'z'`, and the mirror of `'y'` is `'b'`.

Initially, all characters in the string `s` are **unmarked**.

You start with a score of 0, and you perform the following process on the string `s`:

*   Iterate through the string from left to right.
*   At each index `i`, find the closest **unmarked** index `j` such that `j < i` and `s[j]` is the mirror of `s[i]`. Then, **mark** both indices `i` and `j`, and add the value `i - j` to the total score.
*   If no such index `j` exists for the index `i`, move on to the next index without making any changes.

Return the total score at the end of the process.

**Example 1:**

**Input:** s = "aczzx"

**Output:** 5

**Explanation:**

*   `i = 0`. There is no index `j` that satisfies the conditions, so we skip.
*   `i = 1`. There is no index `j` that satisfies the conditions, so we skip.
*   `i = 2`. The closest index `j` that satisfies the conditions is `j = 0`, so we mark both indices 0 and 2, and then add `2 - 0 = 2` to the score.
*   `i = 3`. There is no index `j` that satisfies the conditions, so we skip.
*   `i = 4`. The closest index `j` that satisfies the conditions is `j = 1`, so we mark both indices 1 and 4, and then add `4 - 1 = 3` to the score.

**Example 2:**

**Input:** s = "abcdef"

**Output:** 0

**Explanation:**

For each index `i`, there is no index `j` that satisfies the conditions.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists only of lowercase English letters.

## Solution

```java
import java.util.ArrayList;

@SuppressWarnings("unchecked")
public class Solution {
    public long calculateScore(String s) {
        int n = s.length();
        ArrayList<Integer>[] st = new ArrayList[26];
        long r = 0;
        for (int i = 0; i < 26; i++) {
            st[i] = new ArrayList<>();
        }
        for (int i = 0; i < n; i++) {
            int mc = 'z' - (s.charAt(i) - 'a');
            int p = mc - 'a';
            if (!st[p].isEmpty()) {
                r += i - st[p].remove(st[p].size() - 1);
            } else {
                st[s.charAt(i) - 'a'].add(i);
            }
        }
        return r;
    }
}
```