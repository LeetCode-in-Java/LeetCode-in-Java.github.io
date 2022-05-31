## 1781\. Sum of Beauty of All Substrings

Medium

The **beauty** of a string is the difference in frequencies between the most frequent and least frequent characters.

*   For example, the beauty of `"abaacc"` is `3 - 1 = 2`.

Given a string `s`, return _the sum of **beauty** of all of its substrings._

**Example 1:**

**Input:** s = "aabcb"

**Output:** 5

**Explanation:** The substrings with non-zero beauty are ["aab","aabc","aabcb","abcb","bcb"], each with beauty equal to 1.

**Example 2:**

**Input:** s = "aabcbaa"

**Output:** 17 

**Constraints:**

*   `1 <= s.length <= 500`
*   `s` consists of only lowercase English letters.

## Solution

```java
public class Solution {
    public int beautySum(String s) {
        int beauty = 0;
        for (int i = 0; i < s.length(); i++) {
            int[] numCountOfFreq = new int[s.length() + 1 - i];
            int[] charFreq = new int[26];
            charFreq[s.charAt(i) - 'a'] = 1;
            numCountOfFreq[1] = 1;
            int min = 1;
            int max = 1;
            for (int j = i + 1; j < s.length(); j++) {
                char c = s.charAt(j);
                charFreq[c - 'a']++;
                int freq = charFreq[c - 'a'];
                numCountOfFreq[freq - 1]--;
                numCountOfFreq[freq]++;
                if (numCountOfFreq[min] == 0) {
                    min++;
                }
                if (min > freq) {
                    min = freq;
                }
                if (max < freq) {
                    max = freq;
                }
                beauty += max - min;
            }
        }
        return beauty;
    }
}
```