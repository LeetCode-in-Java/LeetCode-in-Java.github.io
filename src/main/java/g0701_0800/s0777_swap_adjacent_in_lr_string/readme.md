## 777\. Swap Adjacent in LR String

Medium

In a string composed of `'L'`, `'R'`, and `'X'` characters, like `"RXXLRXRXL"`, a move consists of either replacing one occurrence of `"XL"` with `"LX"`, or replacing one occurrence of `"RX"` with `"XR"`. Given the starting string `start` and the ending string `end`, return `True` if and only if there exists a sequence of moves to transform one string to the other.

**Example 1:**

**Input:** start = "RXXLRXRXL", end = "XRLXXRRLX"

**Output:** true

**Explanation:**

    We can transform start to end following these steps:
    RXXLRXRXL ->
    XRXLRXRXL ->
    XRLXRXRXL ->
    XRLXXRRXL ->
    XRLXXRRLX 

**Example 2:**

**Input:** start = "X", end = "L"

**Output:** false 

**Constraints:**

*   <code>1 <= start.length <= 10<sup>4</sup></code>
*   `start.length == end.length`
*   Both `start` and `end` will only consist of characters in `'L'`, `'R'`, and `'X'`.

## Solution

```java
public class Solution {
    public boolean canTransform(String start, String end) {
        int i1 = 0;
        int i2 = 0;
        char[] s1 = start.toCharArray();
        char[] s2 = end.toCharArray();
        while (i1 < s1.length && i2 < s2.length) {
            while (i1 < s1.length && s1[i1] == 'X') {
                i1++;
            }
            while (i2 < s2.length && s2[i2] == 'X') {
                i2++;
            }
            if (i1 == s1.length && i2 == s2.length) {
                return true;
            }
            if (i1 == s1.length || i2 == s2.length) {
                return false;
            }
            char c1 = s1[i1];
            char c2 = s2[i2];
            if (c1 != c2) {
                return false;
            }
            if (c1 == 'L' && i1 < i2) {
                return false;
            }
            if (c1 == 'R' && i1 > i2) {
                return false;
            }
            i1++;
            i2++;
        }
        while (i1 < s1.length && start.charAt(i1) == 'X') {
            i1++;
        }
        while (i2 < s2.length && end.charAt(i2) == 'X') {
            i2++;
        }
        return i1 == s1.length && i2 == s2.length;
    }
}
```