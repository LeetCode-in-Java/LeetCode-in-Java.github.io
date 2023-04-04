[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2430\. Maximum Deletions on a String

Hard

You are given a string `s` consisting of only lowercase English letters. In one operation, you can:

*   Delete **the entire string** `s`, or
*   Delete the **first** `i` letters of `s` if the first `i` letters of `s` are **equal** to the following `i` letters in `s`, for any `i` in the range `1 <= i <= s.length / 2`.

For example, if `s = "ababc"`, then in one operation, you could delete the first two letters of `s` to get `"abc"`, since the first two letters of `s` and the following two letters of `s` are both equal to `"ab"`.

Return _the **maximum** number of operations needed to delete all of_ `s`.

**Example 1:**

**Input:** s = "abcabcdabc"

**Output:** 2

**Explanation:** 
- Delete the first 3 letters ("abc") since the next 3 letters are equal. Now, s = "abcdabc". 
 
- Delete all the letters. 

We used 2 operations so return 2. It can be proven that 2 is the maximum number of operations needed. 

Note that in the second operation we cannot delete "abc" again because the next occurrence of "abc" does not happen in the next 3 letters.

**Example 2:**

**Input:** s = "aaabaab"

**Output:** 4

**Explanation:** 
- 
- Delete the first letter ("a") since the next letter is equal. Now, s = "aabaab". 

- Delete the first 3 letters ("aab") since the next 3 letters are equal. Now, s = "aab". 

- Delete the first letter ("a") since the next letter is equal. Now, s = "ab".

- Delete all the letters. 

We used 4 operations so return 4. It can be proven that 4 is the maximum number of operations needed.

**Example 3:**

**Input:** s = "aaaaa"

**Output:** 5

**Explanation:** In each operation, we can delete the first letter of s.

**Constraints:**

*   `1 <= s.length <= 4000`
*   `s` consists only of lowercase English letters.

## Solution

```java
import java.util.HashMap;

public class Solution {
    private String s;
    private int[] hash;
    private int[] pows;
    private HashMap<Integer, Integer> visited;

    public int deleteString(String s) {
        this.s = s;
        if (isBad()) {
            return s.length();
        }
        visited = new HashMap<>();
        fill();

        return helper(0, 1, 0, 1);
    }

    private int helper(int a, int b, int id1, int id2) {
        int mask = (id1 << 12) + id2;
        int ans = 1;
        if (visited.containsKey(mask)) {
            return visited.get(mask);
        }
        for (; b < s.length(); a++, id2++, b += 2) {
            if ((hash[a + 1] - hash[id1]) * pows[id2] == (hash[b + 1] - hash[id2]) * pows[id1]) {
                if (id2 + 1 == s.length()) {
                    ans = Math.max(ans, 2);
                } else {
                    ans = Math.max(ans, 1 + helper(id2, id2 + 1, id2, id2 + 1));
                }
            }
        }
        visited.put(mask, ans);
        return ans;
    }

    private void fill() {
        int n = s.length();
        hash = new int[n + 1];
        pows = new int[n];
        pows[0] = 1;
        hash[1] = s.charAt(0);
        for (int i = 1; i != n; i++) {
            int temp = pows[i] = pows[i - 1] * 1000000007;
            hash[i + 1] = s.charAt(i) * temp + hash[i];
        }
    }

    private boolean isBad() {
        int i = 1;
        while (i < s.length()) {
            if (s.charAt(0) != s.charAt(i++)) {
                return false;
            }
        }
        return true;
    }
}
```