[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3\. Longest Substring Without Repeating Characters

Medium

Given a string `s`, find the length of the **longest substring** without repeating characters.

**Example 1:**

**Input:** s = "abcabcbb"

**Output:** 3

**Explanation:** The answer is "abc", with the length of 3. 

**Example 2:**

**Input:** s = "bbbbb"

**Output:** 1

**Explanation:** The answer is "b", with the length of 1. 

**Example 3:**

**Input:** s = "pwwkew"

**Output:** 3

**Explanation:** The answer is "wke", with the length of 3. Notice that the answer must be a substring, "pwke" is a subsequence and not a substring. 

**Example 4:**

**Input:** s = ""

**Output:** 0 

**Constraints:**

*   <code>0 <= s.length <= 5 * 10<sup>4</sup></code>
*   `s` consists of English letters, digits, symbols and spaces.

## Solution

```java
public class Solution {
    public int lengthOfLongestSubstring(String s) {
        int[] lastIndices = new int[256];
        for (int i = 0; i < 256; i++) {
            lastIndices[i] = -1;
        }
        int maxLen = 0;
        int curLen = 0;
        int start = 0;
        for (int i = 0; i < s.length(); i++) {
            char cur = s.charAt(i);
            if (lastIndices[cur] < start) {
                lastIndices[cur] = i;
                curLen++;
            } else {
                int lastIndex = lastIndices[cur];
                start = lastIndex + 1;
                curLen = i - start + 1;
                lastIndices[cur] = i;
            }
            if (curLen > maxLen) {
                maxLen = curLen;
            }
        }
        return maxLen;
    }
}
```

**Time Complexity (Big O Time):**

The time complexity of this program is O(n), where "n" represents the length of the input string `s`. Here's the breakdown:

1. The program iterates through the characters of the input string `s` using a `for` loop, and this loop runs for "n" iterations, where "n" is the length of `s`.
2. Inside the loop, it performs constant-time operations:
   - Initializing the `lastIndices` array (constant time).
   - Updating values in the `lastIndices` array (constant time).
   - Updating variables like `maxLen`, `curLen`, `start`, and `lastIndices[cur]` (constant time).

Since the loop runs for "n" iterations, and all operations inside the loop are constant time, the overall time complexity is O(n).

**Space Complexity (Big O Space):**

The space complexity of this program is O(256) = O(1), as it uses a fixed-size array `lastIndices` of length 256 to store the last indices of characters. This array's size does not depend on the input size and remains constant.

Additionally, the program uses a few integer variables and constant space to store the result. The space used for these variables does not depend on the input size and is also considered constant.

Therefore, the space complexity is O(1), or more precisely, O(256), which is still considered constant because it doesn't grow with the input size.
