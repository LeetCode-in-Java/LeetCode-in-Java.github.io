## 796\. Rotate String

Easy

Given two strings `s` and `goal`, return `true` _if and only if_ `s` _can become_ `goal` _after some number of **shifts** on_ `s`.

A **shift** on `s` consists of moving the leftmost character of `s` to the rightmost position.

*   For example, if `s = "abcde"`, then it will be `"bcdea"` after one shift.

**Example 1:**

**Input:** s = "abcde", goal = "cdeab"

**Output:** true 

**Example 2:**

**Input:** s = "abcde", goal = "abced"

**Output:** false 

**Constraints:**

*   `1 <= s.length, goal.length <= 100`
*   `s` and `goal` consist of lowercase English letters.

## Solution

```java
public class Solution {
    private boolean check(String s, String goal, int i) {
        int j = 0;
        int len = goal.length();
        while (j < len) {
            if (i == len) {
                i = 0;
            }
            if (s.charAt(i) != goal.charAt(j)) {
                return false;
            }
            j++;
            i++;
        }
        return true;
    }

    public boolean rotateString(String s, String goal) {
        if (s.length() != goal.length()) {
            return false;
        }
        int len = s.length();
        if (s.charAt(0) == goal.charAt(0) && !s.equals(goal)) {
            return false;
        }
        for (int i = 0; i < len; i++) {
            if (s.charAt(i) == goal.charAt(0) && check(s, goal, i)) {
                return true;
            }
        }
        return false;
    }
}
```