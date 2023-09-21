[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2781\. Length of the Longest Valid Substring

Hard

You are given a string `word` and an array of strings `forbidden`.

A string is called **valid** if none of its substrings are present in `forbidden`.

Return _the length of the **longest valid substring** of the string_ `word`.

A **substring** is a contiguous sequence of characters in a string, possibly empty.

**Example 1:**

**Input:** word = "cbaaaabc", forbidden = ["aaa","cb"]

**Output:** 4

**Explanation:**

There are 9 valid substrings in word: "c", "b", "a", "ba", "aa", "bc", "baa", "aab", "ab", "abc"and "aabc". The length of the longest valid substring is 4.

It can be shown that all other substrings contain either "aaa" or "cb" as a substring. 

**Example 2:**

**Input:** word = "leetcode", forbidden = ["de","le","e"]

**Output:** 4

**Explanation:**

There are 11 valid substrings in word: "l", "t", "c", "o", "d", "tc", "co", "od", "tco", "cod", and "tcod". The length of the longest valid substring is 4.

It can be shown that all other substrings contain either "de", "le", or "e" as a substring. 

**Constraints:**

*   <code>1 <= word.length <= 10<sup>5</sup></code>
*   `word` consists only of lowercase English letters.
*   <code>1 <= forbidden.length <= 10<sup>5</sup></code>
*   `1 <= forbidden[i].length <= 10`
*   `forbidden[i]` consists only of lowercase English letters.

## Solution

```java
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Solution {
    public int longestValidSubstring(String word, List<String> forbidden) {
        Set<String> set = new HashSet<>();
        for (String s : forbidden) {
            set.add(s);
        }
        int n = word.length();
        int ans = 0;
        int i = 0;
        int j = 0;
        while (j < n) {
            int k = j;
            while (k > j - 10 && k >= i) {
                if (set.contains(word.substring(k, j + 1))) {
                    i = k + 1;
                    break;
                }
                k--;
            }
            ans = Math.max(j - i + 1, ans);
            j++;
        }
        return ans;
    }
}
```