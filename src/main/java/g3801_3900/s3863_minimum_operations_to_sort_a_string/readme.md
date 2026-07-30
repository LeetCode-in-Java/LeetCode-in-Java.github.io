[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3863\. Minimum Operations to Sort a String

Medium

You are given a string `s` consisting of lowercase English letters.

In one operation, you can select any **substring** of `s` that is **not** the entire string and **sort** it in **non-descending alphabetical** order.

Return the **minimum** number of operations required to make `s` sorted in **non-descending** order. If it is not possible, return -1.

**Example 1:**

**Input:** s = "dog"

**Output:** 1

**Explanation:**

*   Sort substring `"og"` to `"go"`.
*   Now, `s = "dgo"`, which is sorted in ascending order. Thus, the answer is 1.

**Example 2:**

**Input:** s = "card"

**Output:** 2

**Explanation:**

*   Sort substring `"car"` to `"acr"`, so `s = "acrd"`.
*   Sort substring `"rd"` to `"dr"`, making `s = "acdr"`, which is sorted in ascending order. Thus, the answer is 2.

**Example 3:**

**Input:** s = "gf"

**Output:** \-1

**Explanation:**

*   It is impossible to sort `s` under the given constraints. Thus, the answer is -1.

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s` consists of only lowercase English letters.

## Solution

```java
public class Solution {
    public int minOperations(String s) {
        int n = s.length();
        if (n == 1) {
            return 0;
        }
        if (n == 2) {
            return s.charAt(0) > s.charAt(1) ? -1 : 0;
        }
        char min = 'z';
        char max = 'a';
        char first = s.charAt(0);
        char last = s.charAt(n - 1);
        char prev = 'a';
        int[] cnt = new int[26];
        boolean sorted = true;
        for (char c : s.toCharArray()) {
            sorted &= prev <= c;
            min = (char) Math.min(min, c);
            max = (char) Math.max(max, c);
            prev = c;
            cnt[c - 'a']++;
        }
        if (sorted) {
            return 0;
        }
        return calculateOperations(first, last, min, max, cnt);
    }

    private int calculateOperations(char first, char last, char min, char max, int[] cnt) {
        if (first == min || last == max) {
            return 1;
        }
        if (first != max || last != min) {
            return 2;
        }
        return cnt[max - 'a'] > 1 || cnt[min - 'a'] > 1 ? 2 : 3;
    }
}
```