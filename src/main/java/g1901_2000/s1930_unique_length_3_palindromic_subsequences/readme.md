## 1930\. Unique Length-3 Palindromic Subsequences

Medium

Given a string `s`, return _the number of **unique palindromes of length three** that are a **subsequence** of_ `s`.

Note that even if there are multiple ways to obtain the same subsequence, it is still only counted **once**.

A **palindrome** is a string that reads the same forwards and backwards.

A **subsequence** of a string is a new string generated from the original string with some characters (can be none) deleted without changing the relative order of the remaining characters.

*   For example, `"ace"` is a subsequence of `"abcde"`.

**Example 1:**

**Input:** s = "aabca"

**Output:** 3

**Explanation:** The 3 palindromic subsequences of length 3 are: 

- "aba" (subsequence of "aabca") 

- "aaa" (subsequence of "aabca") 

- "aca" (subsequence of "aabca")

**Example 2:**

**Input:** s = "adc"

**Output:** 0

**Explanation:** There are no palindromic subsequences of length 3 in "adc".

**Example 3:**

**Input:** s = "bbcbaba"

**Output:** 4

**Explanation:** The 4 palindromic subsequences of length 3 are: 

- "bbb" (subsequence of "bbcbaba") 

- "bcb" (subsequence of "bbcbaba") 

- "bab" (subsequence of "bbcbaba") 

- "aba" (subsequence of "bbcbaba")

**Constraints:**

*   <code>3 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only lowercase English letters.

## Solution

```java
import java.util.Arrays;
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int countPalindromicSubsequence(String s) {
        int[] last = new int[26];
        Arrays.fill(last, -1);
        for (int i = s.length() - 1; i >= 0; i--) {
            if (last[s.charAt(i) - 'a'] == -1) {
                last[s.charAt(i) - 'a'] = i;
            }
        }
        int ans = 0;
        int[] count = new int[26];
        Map<Integer, int[]> first = new HashMap<>();
        for (int i = 0; i < s.length(); i++) {
            int cur = s.charAt(i) - 'a';
            if (last[cur] - i <= 1 && !first.containsKey(cur)) {
                last[cur] = -1;
            }
            if (last[cur] == i) {
                int[] oldCount = first.get(cur);
                for (int j = 0; j < 26; j++) {
                    if (count[j] - oldCount[j] > 0) {
                        ans++;
                    }
                }
            }
            count[cur]++;
            if (last[cur] > -1 && !first.containsKey(cur)) {
                first.put(cur, count.clone());
            }
        }
        return ans;
    }
}
```