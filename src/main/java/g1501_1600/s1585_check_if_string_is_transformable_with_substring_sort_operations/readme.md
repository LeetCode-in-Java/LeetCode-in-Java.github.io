[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1585\. Check If String Is Transformable With Substring Sort Operations

Hard

Given two strings `s` and `t`, transform string `s` into string `t` using the following operation any number of times:

*   Choose a **non-empty** substring in `s` and sort it in place so the characters are in **ascending order**.
    *   For example, applying the operation on the underlined substring in `"14234"` results in `"12344"`.

Return `true` if _it is possible to transform `s` into `t`_. Otherwise, return `false`.

A **substring** is a contiguous sequence of characters within a string.

**Example 1:**

**Input:** s = "84532", t = "34852"

**Output:** true

**Explanation:** You can transform s into t using the following sort operations: "84532" (from index 2 to 3) -> "84352" "84352" (from index 0 to 2) -> "34852"

**Example 2:**

**Input:** s = "34521", t = "23415"

**Output:** true

**Explanation:** You can transform s into t using the following sort operations: "34521" -> "23451" "23451" -> "23415"

**Example 3:**

**Input:** s = "12345", t = "12435"

**Output:** false

**Constraints:**

*   `s.length == t.length`
*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` and `t` consist of only digits.

## Solution

```java
public class Solution {
    public boolean isTransformable(String s, String t) {
        int n = s.length();
        if (n != t.length()) {
            return false;
        }
        int[] cnt = new int[10];
        for (int i = 0; i < n; i++) {
            cnt[s.charAt(i) - '0']++;
        }
        for (int i = 0; i < n; i++) {
            cnt[t.charAt(i) - '0']--;
        }
        for (int i = 0; i < 10; i++) {
            if (cnt[i] != 0) {
                return false;
            }
        }
        int[] sCnt = new int[10];
        int[] tCnt = new int[10];
        for (int i = 0; i < n; i++) {
            int sAsci = s.charAt(i) - '0';
            int tAsci = t.charAt(i) - '0';
            sCnt[sAsci]++;
            if (tCnt[sAsci] >= sCnt[sAsci] || (sAsci == tAsci && tCnt[sAsci] + 1 >= sCnt[sAsci])) {
                int rem = 0;
                for (int j = 0; j < sAsci; j++) {
                    if (sCnt[j] - tCnt[j] > 0) {
                        rem++;
                    }
                }
                if (rem > 0) {
                    return false;
                }
            }
            if (sCnt[tAsci] >= tCnt[tAsci] + 1) {
                int rem = 0;
                for (int j = tAsci; j < 10; j++) {
                    if (tCnt[j] - sCnt[j] > 0) {
                        rem++;
                    }
                }
                if (rem > 0) {
                    return false;
                }
            }
            tCnt[tAsci]++;
        }
        return true;
    }
}
```