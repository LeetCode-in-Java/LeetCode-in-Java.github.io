[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 139\. Word Break

Medium

Given a string `s` and a dictionary of strings `wordDict`, return `true` if `s` can be segmented into a space-separated sequence of one or more dictionary words.

**Note** that the same word in the dictionary may be reused multiple times in the segmentation.

**Example 1:**

**Input:** s = "leetcode", wordDict = ["leet","code"]

**Output:** true

**Explanation:** Return true because "leetcode" can be segmented as "leet code". 

**Example 2:**

**Input:** s = "applepenapple", wordDict = ["apple","pen"]

**Output:** true

**Explanation:** Return true because "applepenapple" can be segmented as "apple pen apple". Note that you are allowed to reuse a dictionary word. 

**Example 3:**

**Input:** s = "catsandog", wordDict = ["cats","dog","sand","and","cat"]

**Output:** false 

**Constraints:**

*   `1 <= s.length <= 300`
*   `1 <= wordDict.length <= 1000`
*   `1 <= wordDict[i].length <= 20`
*   `s` and `wordDict[i]` consist of only lowercase English letters.
*   All the strings of `wordDict` are **unique**.

## Solution

```java
import java.util.List;

public class Solution {
    private Boolean[] memo;

    public boolean wordBreak(String s, List<String> wordDict) {
        memo = new Boolean[s.length() + 1];
        return dp(s, 0, wordDict);
    }

    public boolean dp(String s, int i, List<String> wordDict) {
        if (i == s.length()) {
            return true;
        }
        if (memo[i] != null) {
            return memo[i];
        }
        for (String word : wordDict) {
            int len = word.length();
            if (i + len > s.length() || !s.substring(i, i + len).equals(word)) {
                continue;
            }
            if (dp(s, i + len, wordDict)) {
                memo[i] = true;
                return true;
            }
        }
        memo[i] = false;
        return false;
    }
}
```

**Time Complexity (Big O Time):**

- **HashSet Initialization**: Initializing the `HashSet` with the words from the `wordDict` takes O(M) time, where M is the total length of all words in the dictionary.

- **Maximum Length Calculation**: In the first loop, the program calculates the maximum word length in the `wordDict`. This takes O(M) time, as it iterates through all the words.

- **DFS Function Calls**: The main work is done in the `dfs` function. The function is called multiple times with different `end` values (from 1 to the maximum word length). The number of calls depends on the maximum word length, which we denoted as "max." Therefore, the time complexity of the DFS portion is O(max * N), where N is the length of the input string `s`.

- **Flag Array**: The program uses a `flag` array to memoize subproblem results to avoid redundant work. Initializing this array takes O(N) time since it has a length equal to the length of the input string `s`.

Combining these steps, the overall time complexity of the program is O(M + max * N), where M is the total length of all words in the dictionary, N is the length of the input string `s`, and max is the maximum word length in the dictionary.

**Space Complexity (Big O Space):**

- **HashSet Space**: The `HashSet` `set` is used to store the words from the dictionary. It consumes O(M) space, where M is the total length of all words in the dictionary.

- **Flag Array**: The `flag` array is used for memoization, and it has a length equal to the length of the input string `s`, so it consumes O(N) space.

- **DFS Recursive Calls**: The depth of the recursive calls in the `dfs` function can go up to "max," where "max" is the maximum word length in the dictionary. This contributes to the call stack space. Therefore, in the worst case, the space complexity due to recursive calls is O(max).

Combining these space requirements, the overall space complexity of the program is O(M + N + max).

In summary, the time complexity is O(M + max * N), and the space complexity is O(M + N + max), where M is the total length of all words in the dictionary, N is the length of the input string `s`, and max is the maximum word length in the dictionary.
