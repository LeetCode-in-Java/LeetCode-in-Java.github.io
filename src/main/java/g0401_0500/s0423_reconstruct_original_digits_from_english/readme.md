## 423\. Reconstruct Original Digits from English

Medium

Given a string `s` containing an out-of-order English representation of digits `0-9`, return _the digits in **ascending** order_.

**Example 1:**

**Input:** s = "owoztneoer"

**Output:** "012" 

**Example 2:**

**Input:** s = "fviefuro"

**Output:** "45" 

**Constraints:**

*   <code>1 <= s.length <= 10<sup>5</sup></code>
*   `s[i]` is one of the characters `["e","g","f","i","h","o","n","s","r","u","t","w","v","x","z"]`.
*   `s` is **guaranteed** to be valid.

## Solution

```java
public class Solution {
    public String originalDigits(String s) {
        int[] count = new int[26];
        int[] digits = new int[10];
        StringBuilder str = new StringBuilder();
        for (char c : s.toCharArray()) {
            ++count[c - 'a'];
        }
        digits[0] = count[25];
        digits[2] = count[22];
        digits[4] = count[20];
        digits[6] = count[23];
        digits[8] = count[6];
        digits[1] = count[14] - digits[0] - digits[2] - digits[4];
        digits[3] = count[7] - digits[8];
        digits[5] = count[5] - digits[4];
        digits[7] = count[18] - digits[6];
        digits[9] = count[8] - digits[5] - digits[6] - digits[8];
        for (int i = 0; i < 10; i++) {
            while (digits[i]-- != 0) {
                str.append((char) (i + 48));
            }
        }
        return str.toString();
    }
}
```