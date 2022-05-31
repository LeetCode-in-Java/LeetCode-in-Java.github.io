## 1433\. Check If a String Can Break Another String

Medium

Given two strings: `s1` and `s2` with the same size, check if some permutation of string `s1` can break some permutation of string `s2` or vice-versa. In other words `s2` can break `s1` or vice-versa.

A string `x` can break string `y` (both of size `n`) if `x[i] >= y[i]` (in alphabetical order) for all `i` between `0` and `n-1`.

**Example 1:**

**Input:** s1 = "abc", s2 = "xya"

**Output:** true

**Explanation:** "ayx" is a permutation of s2="xya" which can break to string "abc" which is a permutation of s1="abc".

**Example 2:**

**Input:** s1 = "abe", s2 = "acd"

**Output:** false

**Explanation:** All permutations for s1="abe" are: "abe", "aeb", "bae", "bea", "eab" and "eba" and all permutation for s2="acd" are: "acd", "adc", "cad", "cda", "dac" and "dca". However, there is not any permutation from s1 which can break some permutation from s2 and vice-versa.

**Example 3:**

**Input:** s1 = "leetcodee", s2 = "interview"

**Output:** true

**Constraints:**

*   `s1.length == n`
*   `s2.length == n`
*   `1 <= n <= 10^5`
*   All strings consist of lowercase English letters.

## Solution

```java
import java.util.Objects;

public class Solution {
    public boolean checkIfCanBreak(String s1, String s2) {
        if (s1 == null && s2 == null || Objects.requireNonNull(s1).length() == 1) {
            return true;
        }
        int[] count1 = new int[26];
        int[] count2 = new int[26];

        for (int i = s1.length() - 1; i >= 0; i--) {
            count1[s1.charAt(i) - 'a']++;
            count2[s2.charAt(i) - 'a']++;
        }
        boolean isS1Greater = count1[25] >= count2[25];
        boolean isS2Greater = count2[25] >= count1[25];

        for (int i = 24; (isS1Greater || isS2Greater) && i >= 0; i--) {
            count1[i] += count1[i + 1];
            count2[i] += count2[i + 1];

            isS1Greater = isS1Greater && count1[i] >= count2[i];
            isS2Greater = isS2Greater && count2[i] >= count1[i];
        }
        return isS1Greater || isS2Greater;
    }
}
```