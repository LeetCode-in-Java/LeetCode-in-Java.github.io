[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2451\. Odd String Difference

Easy

You are given an array of equal-length strings `words`. Assume that the length of each string is `n`.

Each string `words[i]` can be converted into a **difference integer array** `difference[i]` of length `n - 1` where `difference[i][j] = words[i][j+1] - words[i][j]` where `0 <= j <= n - 2`. Note that the difference between two letters is the difference between their **positions** in the alphabet i.e. the position of `'a'` is `0`, `'b'` is `1`, and `'z'` is `25`.

*   For example, for the string `"acb"`, the difference integer array is `[2 - 0, 1 - 2] = [2, -1]`.

All the strings in words have the same difference integer array, **except one**. You should find that string.

Return _the string in_ `words` _that has different **difference integer array**._

**Example 1:**

**Input:** words = ["adc","wzy","abc"]

**Output:** "abc"

**Explanation:** 

- The difference integer array of "adc" is [3 - 0, 2 - 3] = [3, -1].

- The difference integer array of "wzy" is [25 - 22, 24 - 25]= [3, -1]. 

- The difference integer array of "abc" is [1 - 0, 2 - 1] = [1, 1].

The odd array out is [1, 1], so we return the corresponding string, "abc".

**Example 2:**

**Input:** words = ["aaa","bob","ccc","ddd"]

**Output:** "bob"

**Explanation:** All the integer arrays are [0, 0] except for "bob", which corresponds to [13, -13].

**Constraints:**

*   `3 <= words.length <= 100`
*   `n == words[i].length`
*   `2 <= n <= 20`
*   `words[i]` consists of lowercase English letters.

## Solution

```java
public class Solution {
    public String oddString(String[] w) {
        int n = w[0].length() - 1;
        int[] x = new int[n];
        int s = 1;
        int y = 0;
        int index = 1;
        for (int i = 0; i < n; i++) {
            x[i] = w[0].charAt(i + 1) - w[0].charAt(i);
        }
        for (int i = 1; y * s == 0 || s + y < 3; i++) {
            boolean b = true;
            for (int j = 0; j < n; j++) {
                if (x[j] != w[i].charAt(j + 1) - w[i].charAt(j)) {
                    b = false;
                    break;
                }
            }
            if (b) {
                s++;
            } else {
                y++;
                index = i;
            }
        }
        return s == 1 ? w[0] : w[index];
    }
}
```