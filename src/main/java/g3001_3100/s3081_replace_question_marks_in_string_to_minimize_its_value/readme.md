[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3081\. Replace Question Marks in String to Minimize Its Value

Medium

You are given a string `s`. `s[i]` is either a lowercase English letter or `'?'`.

For a string `t` having length `m` containing **only** lowercase English letters, we define the function `cost(i)` for an index `i` as the number of characters **equal** to `t[i]` that appeared before it, i.e. in the range `[0, i - 1]`.

The **value** of `t` is the **sum** of `cost(i)` for all indices `i`.

For example, for the string `t = "aab"`:

*   `cost(0) = 0`
*   `cost(1) = 1`
*   `cost(2) = 0`
*   Hence, the value of `"aab"` is `0 + 1 + 0 = 1`.

Your task is to **replace all** occurrences of `'?'` in `s` with any lowercase English letter so that the **value** of `s` is **minimized**.

Return _a string denoting the modified string with replaced occurrences of_ `'?'`_. If there are multiple strings resulting in the **minimum value**, return the lexicographically smallest one._

**Example 1:**

**Input:** s = "???"

**Output:** "abc"

**Explanation:** In this example, we can replace the occurrences of `'?'` to make `s` equal to `"abc"`.

For `"abc"`, `cost(0) = 0`, `cost(1) = 0`, and `cost(2) = 0`.

The value of `"abc"` is `0`.

Some other modifications of `s` that have a value of `0` are `"cba"`, `"abz"`, and, `"hey"`.

Among all of them, we choose the lexicographically smallest.

**Example 2:**

**Input:** s = "a?a?"

**Output:** "abac"

**Explanation:** In this example, the occurrences of `'?'` can be replaced to make `s` equal to `"abac"`.

For `"abac"`, `cost(0) = 0`, `cost(1) = 0`, `cost(2) = 1`, and `cost(3) = 0`.

The value of `"abac"` is `1`.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is either a lowercase English letter or `'?'`.

## Solution

```java
public class Solution {
    public String minimizeStringValue(String s) {
        int n = s.length();
        int time = 0;
        int[] count = new int[26];
        int[] res = new int[26];
        char[] str = s.toCharArray();
        for (char c : str) {
            if (c != '?') {
                count[c - 'a']++;
            } else {
                time++;
            }
        }
        int minTime = Integer.MAX_VALUE;
        for (int i = 0; i < 26; i++) {
            minTime = Math.min(minTime, count[i]);
        }
        while (time > 0) {
            for (int j = 0; j < 26; j++) {
                if (count[j] == minTime) {
                    res[j]++;
                    count[j]++;
                    time--;
                }
                if (time == 0) {
                    break;
                }
            }
            minTime++;
        }
        int start = 0;
        for (int i = 0; i < n; i++) {
            if (str[i] == '?') {
                while (res[start] == 0) {
                    start++;
                }
                str[i] = (char) ('a' + start);
                res[start]--;
            }
        }
        return new String(str);
    }
}
```