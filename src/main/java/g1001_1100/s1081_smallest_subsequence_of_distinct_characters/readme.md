[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1081\. Smallest Subsequence of Distinct Characters

Medium

Given a string `s`, return _the lexicographically smallest subsequence of_ `s` _that contains all the distinct characters of_ `s` _exactly once_.

**Example 1:**

**Input:** s = "bcabc"

**Output:** "abc"

**Example 2:**

**Input:** s = "cbacdcbc"

**Output:** "acdb"

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consists of lowercase English letters.

**Note:** This question is the same as 316: [https://leetcode.com/problems/remove-duplicate-letters/](https://leetcode.com/problems/remove-duplicate-letters/)

## Solution

```java
import java.util.ArrayDeque;
import java.util.Arrays;
import java.util.Deque;

public class Solution {
    public String smallestSubsequence(String s) {
        int n = s.length();
        Deque<Character> stk = new ArrayDeque<>();
        int[] freq = new int[26];
        boolean[] exist = new boolean[26];
        Arrays.fill(exist, false);
        for (char ch : s.toCharArray()) {
            freq[ch - 'a']++;
        }
        for (int i = 0; i < n; i++) {
            char ch = s.charAt(i);
            freq[ch - 'a']--;
            if (exist[ch - 'a']) {
                continue;
            }
            while (!stk.isEmpty() && stk.peek() > ch && freq[stk.peek() - 'a'] > 0) {
                char rem = stk.pop();
                exist[rem - 'a'] = false;
            }
            stk.push(ch);
            exist[ch - 'a'] = true;
        }
        char[] ans = new char[stk.size()];
        int index = 0;
        while (!stk.isEmpty()) {
            ans[index] = stk.pop();
            index++;
        }
        return new StringBuilder(new String(ans)).reverse().toString();
    }
}
```