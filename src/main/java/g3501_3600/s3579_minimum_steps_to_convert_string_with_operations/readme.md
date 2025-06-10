[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3579\. Minimum Steps to Convert String with Operations

Hard

You are given two strings, `word1` and `word2`, of equal length. You need to transform `word1` into `word2`.

For this, divide `word1` into one or more **contiguous **substring****. For each substring `substr` you can perform the following operations:

1.  **Replace:** Replace the character at any one index of `substr` with another lowercase English letter.
    
2.  **Swap:** Swap any two characters in `substr`.
    
3.  **Reverse Substring:** Reverse `substr`.
    

Each of these counts as **one** operation and each character of each substring can be used in each type of operation at most once (i.e. no single index may be involved in more than one replace, one swap, or one reverse).

Return the **minimum number of operations** required to transform `word1` into `word2`.

**Example 1:**

**Input:** word1 = "abcdf", word2 = "dacbe"

**Output:** 4

**Explanation:**

Divide `word1` into `"ab"`, `"c"`, and `"df"`. The operations are:

*   For the substring `"ab"`,
    *   Perform operation of type 3 on `"ab" -> "ba"`.
    *   Perform operation of type 1 on `"ba" -> "da"`.
*   For the substring `"c"` do no operations.
*   For the substring `"df"`,
    *   Perform operation of type 1 on `"df" -> "bf"`.
    *   Perform operation of type 1 on `"bf" -> "be"`.

**Example 2:**

**Input:** word1 = "abceded", word2 = "baecfef"

**Output:** 4

**Explanation:**

Divide `word1` into `"ab"`, `"ce"`, and `"ded"`. The operations are:

*   For the substring `"ab"`,
    *   Perform operation of type 2 on `"ab" -> "ba"`.
*   For the substring `"ce"`,
    *   Perform operation of type 2 on `"ce" -> "ec"`.
*   For the substring `"ded"`,
    *   Perform operation of type 1 on `"ded" -> "fed"`.
    *   Perform operation of type 1 on `"fed" -> "fef"`.

**Example 3:**

**Input:** word1 = "abcdef", word2 = "fedabc"

**Output:** 2

**Explanation:**

Divide `word1` into `"abcdef"`. The operations are:

*   For the substring `"abcdef"`,
    *   Perform operation of type 3 on `"abcdef" -> "fedcba"`.
    *   Perform operation of type 2 on `"fedcba" -> "fedabc"`.

**Constraints:**

*   `1 <= word1.length == word2.length <= 100`
*   `word1` and `word2` consist only of lowercase English letters.

## Solution

```java
public class Solution {
    public int minOperations(String word1, String word2) {
        int[] dp = new int[word1.length()];
        int[][] count = new int[26][26];
        for (int i = 0; i < word1.length(); i++) {
            dp[i] = Integer.MAX_VALUE;
        }
        for (int i = 0; i < word1.length(); i++) {
            for (int j = i; j >= 0; j--) {
                int c1 = 0;
                int c2 = 0;
                for (int k1 = j, k2 = j; k1 <= i && k2 <= i; k1++, k2++) {
                    int[] ints = count[word2.charAt(k2) - 'a'];
                    if (ints[word1.charAt(k1) - 'a'] > 0) {
                        ints[word1.charAt(k1) - 'a']--;
                    } else if (word1.charAt(k1) != word2.charAt(k2)) {
                        count[word1.charAt(k1) - 'a'][word2.charAt(k2) - 'a']++;
                        c1++;
                    }
                }
                for (int k1 = j, k2 = j; k1 <= i && k2 <= i; k1++, k2++) {
                    count[word1.charAt(k1) - 'a'][word2.charAt(k2) - 'a'] = 0;
                }
                dp[i] = Math.min(dp[i], j - 1 < 0 ? c1 : dp[j - 1] + c1);
                for (int k1 = j, k2 = i; k1 <= i && k2 >= j; k1++, k2--) {
                    int[] ints = count[word2.charAt(k2) - 'a'];
                    if (ints[word1.charAt(k1) - 'a'] > 0) {
                        ints[word1.charAt(k1) - 'a']--;
                    } else if (word1.charAt(k1) - 'a' != word2.charAt(k2) - 'a') {
                        count[word1.charAt(k1) - 'a'][word2.charAt(k2) - 'a']++;
                        c2++;
                    }
                }
                for (int k1 = j, k2 = i; k1 <= i && k2 >= j; k1++, k2--) {
                    count[word1.charAt(k1) - 'a'][word2.charAt(k2) - 'a'] = 0;
                }
                dp[i] = Math.min(dp[i], j - 1 < 0 ? c2 + 1 : dp[j - 1] + c2 + 1);
            }
        }
        return dp[word1.length() - 1];
    }
}
```