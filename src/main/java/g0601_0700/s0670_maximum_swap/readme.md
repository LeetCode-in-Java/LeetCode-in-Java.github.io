[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 670\. Maximum Swap

Medium

You are given an integer `num`. You can swap two digits at most once to get the maximum valued number.

Return _the maximum valued number you can get_.

**Example 1:**

**Input:** num = 2736

**Output:** 7236

**Explanation:** Swap the number 2 and the number 7.

**Example 2:**

**Input:** num = 9973

**Output:** 9973

**Explanation:** No swap.

**Constraints:**

*   <code>0 <= num <= 10<sup>8</sup></code>

## Solution

```java
public class Solution {
    public int maximumSwap(int num) {
        char[] chars = String.valueOf(num).toCharArray();
        for (int i = 0; i < chars.length; i++) {
            int j = chars.length - 1;
            int indx = i;
            char c = chars[i];
            while (j > i) {
                if (chars[j] > c) {
                    c = chars[j];
                    indx = j;
                }
                j--;
            }
            if (indx != i) {
                char temp = chars[i];
                chars[i] = chars[indx];
                chars[indx] = temp;
                return Integer.parseInt(String.valueOf(chars));
            }
        }
        return num;
    }
}
```