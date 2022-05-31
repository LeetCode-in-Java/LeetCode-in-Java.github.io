## 816\. Ambiguous Coordinates

Medium

We had some 2-dimensional coordinates, like `"(1, 3)"` or `"(2, 0.5)"`. Then, we removed all commas, decimal points, and spaces and ended up with the string s.

*   For example, `"(1, 3)"` becomes `s = "(13)"` and `"(2, 0.5)"` becomes `s = "(205)"`.

Return _a list of strings representing all possibilities for what our original coordinates could have been_.

Our original representation never had extraneous zeroes, so we never started with numbers like `"00"`, `"0.0"`, `"0.00"`, `"1.0"`, `"001"`, `"00.01"`, or any other number that can be represented with fewer digits. Also, a decimal point within a number never occurs without at least one digit occurring before it, so we never started with numbers like `".1"`.

The final answer list can be returned in any order. All coordinates in the final answer have exactly one space between them (occurring after the comma.)

**Example 1:**

**Input:** s = "(123)"

**Output:** ["(1, 2.3)","(1, 23)","(1.2, 3)","(12, 3)"]

**Example 2:**

**Input:** s = "(0123)"

**Output:** ["(0, 1.23)","(0, 12.3)","(0, 123)","(0.1, 2.3)","(0.1, 23)","(0.12, 3)"]

**Explanation:** 0.0, 00, 0001 or 00.01 are not allowed.

**Example 3:**

**Input:** s = "(00011)"

**Output:** ["(0, 0.011)","(0.001, 1)"]

**Constraints:**

*   `4 <= s.length <= 12`
*   `s[0] == '('` and `s[s.length - 1] == ')'`.
*   The rest of `s` are digits.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("java:S107")
public class Solution {
    public List<String> ambiguousCoordinates(String s) {
        char[] sc = s.toCharArray();
        List<String> result = new ArrayList<>();
        StringBuilder sb = new StringBuilder();
        for (int commaPos = 2; commaPos < sc.length - 1; commaPos++) {
            if (isValidNum(sc, 1, commaPos - 1)) {
                if (isValidNum(sc, commaPos, sc.length - 2)) {
                    buildNums(result, sb, sc, commaPos - 1, 0, commaPos, sc.length - 2, 0);
                }
                for (int dp2Idx = commaPos + 1; dp2Idx < sc.length - 1; dp2Idx++) {
                    if (isValidDPNum(sc, commaPos, sc.length - 2, dp2Idx)) {
                        buildNums(result, sb, sc, commaPos - 1, 0, commaPos, sc.length - 2, dp2Idx);
                    }
                }
            }
            for (int dp1Idx = 2; dp1Idx < commaPos; dp1Idx++) {
                if (isValidDPNum(sc, 1, commaPos - 1, dp1Idx)) {
                    if (isValidNum(sc, commaPos, sc.length - 2)) {
                        buildNums(result, sb, sc, commaPos - 1, dp1Idx, commaPos, sc.length - 2, 0);
                    }
                    for (int dp2Idx = commaPos + 1; dp2Idx < sc.length - 1; dp2Idx++) {
                        if (isValidDPNum(sc, commaPos, sc.length - 2, dp2Idx)) {
                            buildNums(
                                    result,
                                    sb,
                                    sc,
                                    commaPos - 1,
                                    dp1Idx,
                                    commaPos,
                                    sc.length - 2,
                                    dp2Idx);
                        }
                    }
                }
            }
        }
        return result;
    }

    private boolean isValidNum(char[] sc, int startIdx, int lastIdx) {
        return sc[startIdx] != '0' || lastIdx - startIdx == 0;
    }

    private boolean isValidDPNum(char[] sc, int startIdx, int lastIdx, int dpIdx) {
        return (sc[startIdx] != '0' || dpIdx - startIdx == 1) && sc[lastIdx] != '0';
    }

    private void buildNums(
            List<String> result,
            StringBuilder sb,
            char[] sc,
            int last1Idx,
            int dp1Idx,
            int start2Idx,
            int last2Idx,
            int dp2Idx) {
        sb.setLength(0);
        sb.append('(');
        if (dp1Idx == 0) {
            sb.append(sc, 1, last1Idx - 1 + 1);
        } else {
            sb.append(sc, 1, dp1Idx - 1).append('.').append(sc, dp1Idx, last1Idx - dp1Idx + 1);
        }
        sb.append(',').append(' ');
        if (dp2Idx == 0) {
            sb.append(sc, start2Idx, last2Idx - start2Idx + 1);
        } else {
            sb.append(sc, start2Idx, dp2Idx - start2Idx)
                    .append('.')
                    .append(sc, dp2Idx, last2Idx - dp2Idx + 1);
        }
        sb.append(')');
        result.add(sb.toString());
    }
}
```