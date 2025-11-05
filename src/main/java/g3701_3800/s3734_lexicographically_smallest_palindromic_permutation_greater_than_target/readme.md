[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3734\. Lexicographically Smallest Palindromic Permutation Greater Than Target

Hard

You are given two strings `s` and `target`, each of length `n`, consisting of lowercase English letters.

Return the **lexicographically smallest string** that is **both** a **palindromic permutation** of `s` and **strictly** greater than `target`. If no such permutation exists, return an empty string.

**Example 1:**

**Input:** s = "baba", target = "abba"

**Output:** "baab"

**Explanation:**

*   The palindromic permutations of `s` (in lexicographical order) are `"abba"` and `"baab"`.
*   The lexicographically smallest permutation that is strictly greater than `target` is `"baab"`.

**Example 2:**

**Input:** s = "baba", target = "bbaa"

**Output:** ""

**Explanation:**

*   The palindromic permutations of `s` (in lexicographical order) are `"abba"` and `"baab"`.
*   None of them is lexicographically strictly greater than `target`. Therefore, the answer is `""`.

**Example 3:**

**Input:** s = "abc", target = "abb"

**Output:** ""

**Explanation:**

`s` has no palindromic permutations. Therefore, the answer is `""`.

**Example 4:**

**Input:** s = "aac", target = "abb"

**Output:** "aca"

**Explanation:**

*   The only palindromic permutation of `s` is `"aca"`.
*   `"aca"` is strictly greater than `target`. Therefore, the answer is `"aca"`.

**Constraints:**

*   `1 <= n == s.length == target.length <= 300`
*   `s` and `target` consist of only lowercase English letters.

## Solution

```java
import java.util.Arrays;

public class Solution {
    boolean func(int i, String target, char[] ans, int l, int r, int[] freq, boolean end) {
        if (l > r) {
            return new String(ans).compareTo(target) > 0;
        }
        if (l == r) {
            char left = '#';
            for (int k = 0; k < 26; k++) {
                if (freq[k] > 0) {
                    left = (char) (k + 'a');
                }
            }
            freq[left - 'a']--;
            ans[l] = left;
            if (func(i + 1, target, ans, l + 1, r - 1, freq, end)) {
                return true;
            }
            freq[left - 'a']++;
            ans[l] = '#';
            return false;
        }
        if (end) {
            for (int j = 0; j < 26; j++) {
                if (freq[j] > 1) {
                    freq[j] -= 2;
                    ans[l] = ans[r] = (char) (j + 'a');
                    if (func(i + 1, target, ans, l + 1, r - 1, freq, end)) {
                        return true;
                    }
                    ans[l] = ans[r] = '#';
                    freq[j] += 2;
                }
            }
            return false;
        }
        char curr = target.charAt(i);
        char next = '1';
        for (int k = target.charAt(i) - 'a' + 1; k < 26; k++) {
            if (freq[k] > 1) {
                next = (char) (k + 'a');
                break;
            }
        }
        if (freq[curr - 'a'] > 1) {
            ans[l] = ans[r] = curr;
            freq[curr - 'a'] -= 2;
            if (func(i + 1, target, ans, l + 1, r - 1, freq, end)) {
                return true;
            }
            freq[curr - 'a'] += 2;
        }
        if (next != '1') {
            ans[l] = ans[r] = next;
            freq[next - 'a'] -= 2;
            if (func(i + 1, target, ans, l + 1, r - 1, freq, true)) {
                return true;
            }
        }
        ans[l] = ans[r] = '#';
        return false;
    }

    public String lexPalindromicPermutation(String s, String target) {
        int[] freq = new int[26];
        for (int i = 0; i < s.length(); i++) {
            freq[s.charAt(i) - 'a']++;
        }
        int oddc = 0;
        for (int i = 0; i < 26; i++) {
            if (freq[i] % 2 == 1) {
                oddc++;
            }
        }
        if (oddc > 1) {
            return "";
        }
        char[] ans = new char[s.length()];
        Arrays.fill(ans, '#');
        if (func(0, target, ans, 0, s.length() - 1, freq, false)) {
            return new String(ans);
        }
        return "";
    }
}
```