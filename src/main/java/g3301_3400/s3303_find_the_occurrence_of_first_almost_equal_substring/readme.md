[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3303\. Find the Occurrence of First Almost Equal Substring

Hard

You are given two strings `s` and `pattern`.

A string `x` is called **almost equal** to `y` if you can change **at most** one character in `x` to make it _identical_ to `y`.

Return the **smallest** _starting index_ of a substring in `s` that is **almost equal** to `pattern`. If no such index exists, return `-1`.

A **substring** is a contiguous **non-empty** sequence of characters within a string.

**Example 1:**

**Input:** s = "abcdefg", pattern = "bcdffg"

**Output:** 1

**Explanation:**

The substring `s[1..6] == "bcdefg"` can be converted to `"bcdffg"` by changing `s[4]` to `"f"`.

**Example 2:**

**Input:** s = "ababbababa", pattern = "bacaba"

**Output:** 4

**Explanation:**

The substring `s[4..9] == "bababa"` can be converted to `"bacaba"` by changing `s[6]` to `"c"`.

**Example 3:**

**Input:** s = "abcd", pattern = "dba"

**Output:** \-1

**Example 4:**

**Input:** s = "dde", pattern = "d"

**Output:** 0

**Constraints:**

*   <code>1 <= pattern.length < s.length <= 3 * 10<sup>5</sup></code>
*   `s` and `pattern` consist only of lowercase English letters.

**Follow-up:** Could you solve the problem if **at most** `k` **consecutive** characters can be changed?

## Solution

```java
public class Solution {
    public int minStartingIndex(String s, String pattern) {
        int n = s.length();
        int left = 0;
        int right = 0;
        int[] f1 = new int[26];
        int[] f2 = new int[26];
        for (char ch : pattern.toCharArray()) {
            f2[ch - 'a']++;
        }
        while (right < n) {
            char ch = s.charAt(right);
            f1[ch - 'a']++;
            if (right - left + 1 == pattern.length() + 1) {
                f1[s.charAt(left) - 'a']--;
                left += 1;
            }
            if (right - left + 1 == pattern.length() && check(f1, f2, left, s, pattern)) {
                return left;
            }
            right += 1;
        }
        return -1;
    }

    private boolean check(int[] f1, int[] f2, int left, String s, String pattern) {
        int cnt = 0;
        for (int i = 0; i < 26; i++) {
            if (f1[i] != f2[i]) {
                if ((Math.abs(f1[i] - f2[i]) > 1) || (Math.abs(f1[i] - f2[i]) != 1 && cnt == 2)) {
                    return false;
                }
                cnt += 1;
            }
        }
        cnt = 0;
        int start = 0;
        int end = pattern.length() - 1;
        while (start <= end) {
            if (s.charAt(start + left) != pattern.charAt(start)) {
                cnt += 1;
            }
            if (start + left != left + end && s.charAt(left + end) != pattern.charAt(end)) {
                cnt += 1;
            }
            if (cnt >= 2) {
                return false;
            }
            start++;
            end--;
        }
        return true;
    }
}
```