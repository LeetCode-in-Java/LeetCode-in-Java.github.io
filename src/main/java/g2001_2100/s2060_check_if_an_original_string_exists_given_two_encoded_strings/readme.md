## 2060\. Check if an Original String Exists Given Two Encoded Strings

Hard

An original string, consisting of lowercase English letters, can be encoded by the following steps:

*   Arbitrarily **split** it into a **sequence** of some number of **non-empty** substrings.
*   Arbitrarily choose some elements (possibly none) of the sequence, and **replace** each with **its length** (as a numeric string).
*   **Concatenate** the sequence as the encoded string.

For example, **one way** to encode an original string `"abcdefghijklmnop"` might be:

*   Split it as a sequence: `["ab", "cdefghijklmn", "o", "p"]`.
*   Choose the second and third elements to be replaced by their lengths, respectively. The sequence becomes `["ab", "12", "1", "p"]`.
*   Concatenate the elements of the sequence to get the encoded string: `"ab121p"`.

Given two encoded strings `s1` and `s2`, consisting of lowercase English letters and digits `1-9` (inclusive), return `true` _if there exists an original string that could be encoded as **both**_ `s1` _and_ `s2`_. Otherwise, return_ `false`.

**Note**: The test cases are generated such that the number of consecutive digits in `s1` and `s2` does not exceed `3`.

**Example 1:**

**Input:** s1 = "internationalization", s2 = "i18n"

**Output:** true

**Explanation:** It is possible that "internationalization" was the original string. 

- "internationalization" 
  
    -> Split: ["internationalization"] 
  
    -> Do not replace any element 
    
    -> Concatenate: "internationalization", which is s1. 
    
- "internationalization" 
  
    -> Split: ["i", "nternationalizatio", "n"]
  
    -> Replace: ["i", "18", "n"] 
  
    -> Concatenate: "i18n", which is s2

**Example 2:**

**Input:** s1 = "l123e", s2 = "44"

**Output:** true

**Explanation:** It is possible that "leetcode" was the original string. 

- "leetcode" 
  
    -> Split: ["l", "e", "et", "cod", "e"] 
  
    -> Replace: ["l", "1", "2", "3", "e"] 
  
    -> Concatenate: "l123e", which is s1. 
    
- "leetcode" 
  
    -> Split: ["leet", "code"] 
  
    -> Replace: ["4", "4"] 
  
    -> Concatenate: "44", which is s2.

**Example 3:**

**Input:** s1 = "a5b", s2 = "c5b"

**Output:** false

**Explanation:** It is impossible. 

- The original string encoded as s1 must start with the letter 'a'. 

- The original string encoded as s2 must start with the letter 'c'.

**Constraints:**

*   `1 <= s1.length, s2.length <= 40`
*   `s1` and `s2` consist of digits `1-9` (inclusive), and lowercase English letters only.
*   The number of consecutive digits in `s1` and `s2` does not exceed `3`.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private boolean stringMatched = false;
    private String s1;
    private String s2;

    static class Key {
        int i1;
        int i2;

        Key(int i1, int i2) {
            this.i1 = i1;
            this.i2 = i2;
        }
    }

    private Boolean[][][] memo;

    public boolean possiblyEquals(String s1, String s2) {
        this.s1 = s1;
        this.s2 = s2;
        memo = new Boolean[s1.length() + 1][s2.length() + 1][2000];
        dfs(0, 0, 0);
        return stringMatched;
    }

    private void dfs(int i1, int i2, int diff) {
        if (stringMatched) {
            return;
        }
        if (i1 == s1.length() && i2 == s2.length()) {
            if (diff == 0) {
                stringMatched = true;
            }
            return;
        }
        if (i1 == s1.length() && diff <= 0) {
            return;
        }
        if (i2 == s2.length() && diff >= 0) {
            return;
        }
        if (memo[i1][i2][diff + 999] != null) {
            stringMatched = memo[i1][i2][diff + 999];
            return;
        }
        List<int[]> indexNums1 = new ArrayList<>();
        int num1 = 0;
        int x1 = i1;
        while (x1 < s1.length() && Character.isDigit(s1.charAt(x1))) {
            num1 = num1 * 10 + (s1.charAt(x1) - '0');
            indexNums1.add(new int[] {x1, num1});
            x1++;
        }
        List<int[]> indexNums2 = new ArrayList<>();
        int num2 = 0;
        int x2 = i2;
        while (x2 < s2.length() && Character.isDigit(s2.charAt(x2))) {
            num2 = num2 * 10 + (s2.charAt(x2) - '0');
            indexNums2.add(new int[] {x2, num2});
            x2++;
        }
        if (diff == 0) {
            char c1 = s1.charAt(i1);
            char c2 = s2.charAt(i2);
            if (Character.isLetter(c1) && Character.isLetter(c2)) {
                if (c1 != c2) {
                    return;
                }
                dfs(i1 + 1, i2 + 1, diff);
                return;
            } else {
                if (!indexNums1.isEmpty()) {
                    for (int[] num1Item : indexNums1) {
                        dfs(num1Item[0] + 1, i2, diff + num1Item[1]);
                    }
                } else {
                    for (int[] num2Item : indexNums2) {
                        dfs(i1, num2Item[0] + 1, diff - num2Item[1]);
                    }
                }
            }
        } else if (diff > 0) {
            if (Character.isLetter(s2.charAt(i2))) {
                dfs(i1, i2 + 1, diff - 1);
            } else {
                for (int[] num2Item : indexNums2) {
                    dfs(i1, num2Item[0] + 1, diff - num2Item[1]);
                }
            }
        } else {
            if (Character.isLetter(s1.charAt(i1))) {
                dfs(i1 + 1, i2, diff + 1);
            } else {
                for (int[] num1Item : indexNums1) {
                    dfs(num1Item[0] + 1, i2, diff + num1Item[1]);
                }
            }
        }
        memo[i1][i2][diff + 999] = stringMatched;
    }
}
```