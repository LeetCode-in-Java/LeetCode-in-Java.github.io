[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 5\. Longest Palindromic Substring

Medium

Given a string `s`, return _the longest palindromic substring_ in `s`.

**Example 1:**

**Input:** s = "babad"

**Output:** "bab" **Note:** "aba" is also a valid answer. 

**Example 2:**

**Input:** s = "cbbd"

**Output:** "bb" 

**Example 3:**

**Input:** s = "a"

**Output:** "a" 

**Example 4:**

**Input:** s = "ac"

**Output:** "a" 

**Constraints:**

*   `1 <= s.length <= 1000`
*   `s` consist of only digits and English letters.

## Solution

```java
public class Solution {
    public String longestPalindrome(String s) {
        char[] newStr = new char[s.length() * 2 + 1];
        newStr[0] = '#';
        for (int i = 0; i < s.length(); i++) {
            newStr[2 * i + 1] = s.charAt(i);
            newStr[2 * i + 2] = '#';
        }
        int[] dp = new int[newStr.length];
        int friendCenter = 0;
        int friendRadius = 0;
        int lpsCenter = 0;
        int lpsRadius = 0;
        for (int i = 0; i < newStr.length; i++) {
            dp[i] =
                    friendCenter + friendRadius > i
                            ? Math.min(dp[friendCenter * 2 - i], (friendCenter + friendRadius) - i)
                            : 1;
            while (i + dp[i] < newStr.length
                    && i - dp[i] >= 0
                    && newStr[i + dp[i]] == newStr[i - dp[i]]) {
                dp[i]++;
            }
            if (friendCenter + friendRadius < i + dp[i]) {
                friendCenter = i;
                friendRadius = dp[i];
            }
            if (lpsRadius < dp[i]) {
                lpsCenter = i;
                lpsRadius = dp[i];
            }
        }
        return s.substring((lpsCenter - lpsRadius + 1) / 2, (lpsCenter + lpsRadius - 1) / 2);
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(n), where "n" represents the length of the input string `s`. Here's the breakdown:

1. The program first creates a new string `newStr` by inserting '#' characters between each character in the original string `s`. This operation has a time complexity of O(n) because it processes each character of `s`.

2. After creating `newStr`, the program uses the Manacher's Algorithm to find the longest palindromic substring. This algorithm is known to have a linear time complexity of O(n).

3. The loop that implements Manacher's Algorithm iterates through the characters of `newStr`. In each iteration, it performs constant-time operations like comparisons and arithmetic calculations. The key operation within this loop is the palindrome expansion, which extends the palindrome around the current center. The loop iterates through `newStr` only once, and each expansion step performs constant work.

Therefore, the overall time complexity of the program is dominated by the linear-time Manacher's Algorithm, making it O(n).

**Space Complexity (Big O Space):**

The space complexity of this program is O(n), where "n" represents the length of the input string `s`. Here's why:

1. The program creates a new character array `newStr` with a length of `s.length() * 2 + 1`. This array stores a modified version of the input string `s` with '#' characters inserted. The space used by `newStr` is proportional to the length of `s`, so it has a space complexity of O(n).

2. The program uses an integer array `dp` of the same length as `newStr` to store information about the lengths of palindromes centered at different positions. The space complexity of `dp` is also O(n).

3. The program uses several integer variables (`friendCenter`, `friendRadius`, `lpsCenter`, and `lpsRadius`) to keep track of information during the algorithm. These variables take up constant space.

Therefore, the overall space complexity is dominated by the space used for `newStr` and `dp`, both of which are O(n).
