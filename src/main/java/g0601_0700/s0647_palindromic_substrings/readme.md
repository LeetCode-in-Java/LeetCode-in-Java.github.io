[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 647\. Palindromic Substrings

Medium

Given a string `s`, return _the number of **palindromic substrings** in it_.

A string is a **palindrome** when it reads the same backward as forward.

A **substring** is a contiguous sequence of characters within the string.

**Example 1:**

**Input:** s = "abc"

**Output:** 3

**Explanation:** Three palindromic strings: "a", "b", "c". 

**Example 2:**

**Input:** s = "aaa"

**Output:** 6

**Explanation:** Six palindromic strings: "a", "a", "a", "aa", "aa", "aaa". 

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consists of lowercase English letters.

## Solution

```java
public class Solution {
    private void expand(char[] a, int l, int r, int[] res) {
        while (l >= 0 && r < a.length) {
            if (a[l] != a[r]) {
                return;
            } else {
                res[0]++;
                l--;
                r++;
            }
        }
    }

    public int countSubstrings(String s) {
        char[] a = s.toCharArray();
        int[] res = {0};
        for (int i = 0; i < a.length; i++) {
            expand(a, i, i, res);
            expand(a, i, i + 1, res);
        }
        return res[0];
    }
}
```

**Time Complexity (Big O Time):**

The program uses a loop that iterates through each character in the input string `s`. For each character, it calls the `expand` function twice:

1. The `expand` function expands around a single character, so it has a linear time complexity of O(n), where 'n' is the length of the input string `s`.

2. The `expand` function expands around a pair of characters (centered at `i` and `i+1`), so it also has a linear time complexity of O(n).

Since both expansions are performed for each character in the input string `s`, the overall time complexity of the program is O(n^2), where 'n' is the length of the input string.

**Space Complexity (Big O Space):**

The space complexity of the program is determined by the additional data structures used, including the character array `a` and the integer array `res`.

1. The character array `a` stores a copy of the input string `s`, which requires O(n) space, where 'n' is the length of the input string.

2. The integer array `res` is used to store the result, and it requires constant space, O(1).

Therefore, the overall space complexity of the program is O(n) due to the character array `a`.

In summary, the provided program has a time complexity of O(n^2) and a space complexity of O(n), where 'n' is the length of the input string `s`. It efficiently counts the number of palindromic substrings by expanding around each character and each pair of characters in the string.
