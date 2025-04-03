[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3499\. Maximize Active Section with Trade I

Medium

You are given a binary string `s` of length `n`, where:

*   `'1'` represents an **active** section.
*   `'0'` represents an **inactive** section.

You can perform **at most one trade** to maximize the number of active sections in `s`. In a trade, you:

*   Convert a contiguous block of `'1'`s that is surrounded by `'0'`s to all `'0'`s.
*   Afterward, convert a contiguous block of `'0'`s that is surrounded by `'1'`s to all `'1'`s.

Return the **maximum** number of active sections in `s` after making the optimal trade.

**Note:** Treat `s` as if it is **augmented** with a `'1'` at both ends, forming `t = '1' + s + '1'`. The augmented `'1'`s **do not** contribute to the final count.

**Example 1:**

**Input:** s = "01"

**Output:** 1

**Explanation:**

Because there is no block of `'1'`s surrounded by `'0'`s, no valid trade is possible. The maximum number of active sections is 1.

**Example 2:**

**Input:** s = "0100"

**Output:** 4

**Explanation:**

*   String `"0100"` → Augmented to `"101001"`.
*   Choose `"0100"`, convert <code>"10<ins>**1**</ins>001"</code> → <code>"1<ins>**0000**</ins>1"</code> → <code>"1<ins>**1111**</ins>1"</code>.
*   The final string without augmentation is `"1111"`. The maximum number of active sections is 4.

**Example 3:**

**Input:** s = "1000100"

**Output:** 7

**Explanation:**

*   String `"1000100"` → Augmented to `"110001001"`.
*   Choose `"000100"`, convert <code>"11000<ins>**1**</ins>001"</code> → <code>"11<ins>**000000**</ins>1"</code> → <code>"11<ins>**111111**</ins>1"</code>.
*   The final string without augmentation is `"1111111"`. The maximum number of active sections is 7.

**Example 4:**

**Input:** s = "01010"

**Output:** 4

**Explanation:**

*   String `"01010"` → Augmented to `"1010101"`.
*   Choose `"010"`, convert <code>"10<ins>**1**</ins>0101"</code> → <code>"1<ins>**000**</ins>101"</code> → <code>"1<ins>**111**</ins>101"</code>.
*   The final string without augmentation is `"11110"`. The maximum number of active sections is 4.

**Constraints:**

*   <code>1 <= n == s.length <= 10<sup>5</sup></code>
*   `s[i]` is either `'0'` or `'1'`

## Solution

```java
public class Solution {
    public int maxActiveSectionsAfterTrade(String s) {
        int oneCount = 0;
        int convertedOne = 0;
        int curZeroCount = 0;
        int lastZeroCount = 0;
        for (int i = 0; i < s.length(); ++i) {
            if (s.charAt(i) == '0') {
                curZeroCount++;
            } else {
                if (curZeroCount != 0) {
                    lastZeroCount = curZeroCount;
                }
                curZeroCount = 0;
                oneCount++;
            }
            convertedOne = Math.max(convertedOne, curZeroCount + lastZeroCount);
        }
        // corner case
        if (convertedOne == curZeroCount || convertedOne == lastZeroCount) {
            return oneCount;
        }
        return oneCount + convertedOne;
    }
}
```