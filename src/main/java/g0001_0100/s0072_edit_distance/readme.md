[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 72\. Edit Distance

Hard

Given two strings `word1` and `word2`, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:

*   Insert a character
*   Delete a character
*   Replace a character

**Example 1:**

**Input:** word1 = "horse", word2 = "ros"

**Output:** 3

**Explanation:** horse -> rorse (replace 'h' with 'r') rorse -> rose (remove 'r') rose -> ros (remove 'e') 

**Example 2:**

**Input:** word1 = "intention", word2 = "execution"

**Output:** 5

**Explanation:** intention -> inention (remove 't') inention -> enention (replace 'i' with 'e') enention -> exention (replace 'n' with 'x') exention -> exection (replace 'n' with 'c') exection -> execution (insert 'u') 

**Constraints:**

*   `0 <= word1.length, word2.length <= 500`
*   `word1` and `word2` consist of lowercase English letters.

## Solution

```java
@SuppressWarnings("java:S2234")
public class Solution {
    public int minDistance(String w1, String w2) {
        int n1 = w1.length();
        int n2 = w2.length();
        if (n2 > n1) {
            return minDistance(w2, w1);
        }
        int[] dp = new int[n2 + 1];
        for (int j = 0; j <= n2; j++) {
            dp[j] = j;
        }
        for (int i = 1; i <= n1; i++) {
            int pre = dp[0];
            dp[0] = i;
            for (int j = 1; j <= n2; j++) {
                int tmp = dp[j];
                dp[j] =
                        w1.charAt(i - 1) != w2.charAt(j - 1)
                                ? 1 + Math.min(pre, Math.min(dp[j], dp[j - 1]))
                                : pre;
                pre = tmp;
            }
        }
        return dp[n2];
    }
}
```

**Time Complexity (Big O Time):**

1. The program uses dynamic programming with a nested loop structure. The outer loop runs from 1 to `n1`, and the inner loop runs from 1 to `n2`, where `n1` and `n2` are the lengths of the input words `w1` and `w2`.

2. Inside the inner loop, there are constant-time operations like array indexing, character comparisons, and mathematical operations (addition and minimum calculations). These operations do not depend on the size of the input words.

3. Therefore, the dominant factor in the time complexity is the nested loops. The outer loop runs for `n1` iterations, and the inner loop runs for `n2` iterations. As a result, the time complexity is O(n1 * n2).

4. In the worst case, `n1` and `n2` can be equal to the lengths of the input words, so the time complexity can be represented as O(n^2), where 'n' is the maximum of the lengths of `w1` and `w2`.

**Space Complexity (Big O Space):**

1. The program uses an integer array `dp` of size `n2 + 1`, where `n2` is the length of the shorter word `w2`. Additionally, there are some integer variables used for temporary storage.

2. Therefore, the space complexity is O(n2) because the space usage is directly proportional to the length of the shorter input word `w2`.

3. The space complexity does not depend on the length of the longer word `w1` because the program is designed to handle the shorter word as the outer loop variable.

In summary, the time complexity of the provided program is O(n^2), where 'n' is the maximum of the lengths of `w1` and `w2`. The space complexity is O(n2), where `n2` is the length of the shorter word `w2`. The program efficiently calculates the minimum edit distance between two words using dynamic programming.
