[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2516\. Take K of Each Character From Left and Right

Medium

You are given a string `s` consisting of the characters `'a'`, `'b'`, and `'c'` and a non-negative integer `k`. Each minute, you may take either the **leftmost** character of `s`, or the **rightmost** character of `s`.

Return _the **minimum** number of minutes needed for you to take **at least**_ `k` _of each character, or return_ `-1` _if it is not possible to take_ `k` _of each character._

**Example 1:**

**Input:** s = "aabaaaacaabc", k = 2

**Output:** 8

**Explanation:**

Take three characters from the left of s. You now have two 'a' characters, and one 'b' character.

Take five characters from the right of s. You now have four 'a' characters, two 'b' characters, and two 'c' characters.

A total of 3 + 5 = 8 minutes is needed.

It can be proven that 8 is the minimum number of minutes needed. 

**Example 2:**

**Input:** s = "a", k = 1

**Output:** -1

**Explanation:** It is not possible to take one 'b' or 'c' so return -1. 

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only the letters `'a'`, `'b'`, and `'c'`.
*   `0 <= k <= s.length`

## Solution

```java
public class Solution {
    public int takeCharacters(String s, int k) {
        if (s.length() < 3 * k) {
            return -1;
        }
        int[] cnt = new int[3];
        char[] arr = s.toCharArray();
        for (char ch : arr) {
            cnt[ch - 'a']++;
        }
        if (cnt[0] < k || cnt[1] < k || cnt[2] < k) {
            return -1;
        }
        int ans = arr.length;
        int i = 0;
        int j = 0;
        while (j < arr.length) {
            cnt[arr[j] - 'a']--;
            if (cnt[0] >= k && cnt[1] >= k && cnt[2] >= k) {
                ans = Math.min(ans, arr.length - (j - i + 1));
                j++;
            } else {
                cnt[arr[i] - 'a']++;
                i++;
                j++;
            }
        }
        return ans;
    }
}
```