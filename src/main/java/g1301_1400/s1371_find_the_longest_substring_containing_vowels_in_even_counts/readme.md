[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1371\. Find the Longest Substring Containing Vowels in Even Counts

Medium

Given the string `s`, return the size of the longest substring containing each vowel an even number of times. That is, 'a', 'e', 'i', 'o', and 'u' must appear an even number of times.

**Example 1:**

**Input:** s = "eleetminicoworoep"

**Output:** 13

**Explanation:** The longest substring is "leetminicowor" which contains two each of the vowels: **e**, **i** and **o** and zero of the vowels: **a** and **u**.

**Example 2:**

**Input:** s = "leetcodeisgreat"

**Output:** 5

**Explanation:** The longest substring is "leetc" which contains two e's.

**Example 3:**

**Input:** s = "bcbcbc"

**Output:** 6

**Explanation:** In this case, the given string "bcbcbc" is the longest because all vowels: **a**, **e**, **i**, **o** and **u** appear zero times.

**Constraints:**

*   `1 <= s.length <= 5 x 10^5`
*   `s` contains only lowercase English letters.

## Solution

```java
import java.util.Arrays;
import java.util.HashSet;
import java.util.Set;

public class Solution {
    private Integer result = null;

    public int findTheLongestSubstring(String s) {
        int[] arr = new int[s.length()];
        int sum = 0;
        Set<Character> set = new HashSet<>(Arrays.asList('a', 'e', 'i', 'o', 'u'));
        for (int i = 0; i < s.length(); i++) {
            char c = s.charAt(i);
            if (set.contains(c)) {
                sum =
                        (sum & (1 << 'a' - c)) == 0
                                ? (sum | (1 << 'a' - c))
                                : (sum & ~(1 << 'a' - c));
            }
            arr[i] = sum;
        }
        for (int i = 0; i < s.length(); i++) {
            if (result != null && result > s.length() - i) {
                break;
            }
            for (int j = s.length() - 1; j >= i; j--) {
                int e = arr[j];
                int k = (i - 1) < 0 ? 0 : arr[i - 1];
                int m = e ^ k;
                if (m == 0) {
                    this.result = result == null ? (j - i) + 1 : Math.max(result, (j - i) + 1);
                    break;
                }
            }
        }
        return result == null ? 0 : result;
    }
}
```