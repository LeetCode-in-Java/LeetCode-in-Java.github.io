[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2953\. Count Complete Substrings

Hard

You are given a string `word` and an integer `k`.

A substring `s` of `word` is **complete** if:

*   Each character in `s` occurs **exactly** `k` times.
*   The difference between two adjacent characters is **at most** `2`. That is, for any two adjacent characters `c1` and `c2` in `s`, the absolute difference in their positions in the alphabet is **at most** `2`.

Return _the number of **complete** substrings of_ `word`.

A **substring** is a **non-empty** contiguous sequence of characters in a string.

**Example 1:**

**Input:** word = "igigee", k = 2

**Output:** 3

**Explanation:** The complete substrings where each character appears exactly twice and the difference between adjacent characters is at most 2 are: <ins>**igig**</ins>ee, igig<ins>**ee**</ins>, <ins>**igigee**</ins>.

**Example 2:**

**Input:** word = "aaabbbccc", k = 3

**Output:** 6

**Explanation:** The complete substrings where each character appears exactly three times and the difference between adjacent characters is at most 2 are: **<ins>aaa</ins>**bbbccc, aaa<ins>**bbb**</ins>ccc, aaabbb<ins>**ccc**</ins>, **<ins>aaabbb</ins>**ccc, aaa<ins>**bbbccc**</ins>, <ins>**aaabbbccc**</ins>.

**Constraints:**

*   <code>1 <= word.length <= 10<sup>5</sup></code>
*   `word` consists only of lowercase English letters.
*   `1 <= k <= word.length`

## Solution

```java
public class Solution {
    public int countCompleteSubstrings(String word, int k) {
        char[] arr = word.toCharArray();
        int n = arr.length;
        int result = 0;
        int last = 0;
        for (int i = 1; i <= n; i++) {
            if (i == n || Math.abs(arr[i] - arr[i - 1]) > 2) {
                result += getCount(arr, k, last, i - 1);
                last = i;
            }
        }
        return result;
    }

    private int getCount(char[] arr, int k, int start, int end) {
        int result = 0;
        for (int i = 1; i <= 26 && i * k <= end - start + 1; i++) {
            int[] cnt = new int[26];
            int good = 0;
            for (int j = start; j <= end; j++) {
                char cR = arr[j];
                cnt[cR - 'a']++;
                if (cnt[cR - 'a'] == k) {
                    good++;
                }
                if (cnt[cR - 'a'] == k + 1) {
                    good--;
                }
                if (j >= start + i * k) {
                    char cL = arr[j - i * k];
                    if (cnt[cL - 'a'] == k) {
                        good--;
                    }
                    if (cnt[cL - 'a'] == k + 1) {
                        good++;
                    }
                    cnt[cL - 'a']--;
                }
                if (good == i) {
                    result++;
                }
            }
        }
        return result;
    }
}
```