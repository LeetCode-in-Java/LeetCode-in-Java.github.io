## 784\. Letter Case Permutation

Medium

Given a string `s`, you can transform every letter individually to be lowercase or uppercase to create another string.

Return _a list of all possible strings we could create_. Return the output in **any order**.

**Example 1:**

**Input:** s = "a1b2"

**Output:** ["a1b2","a1B2","A1b2","A1B2"] 

**Example 2:**

**Input:** s = "3z4"

**Output:** ["3z4","3Z4"] 

**Constraints:**

*   `1 <= s.length <= 12`
*   `s` consists of lowercase English letters, uppercase English letters, and digits.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private List<String> ans = new ArrayList<>();

    public List<String> letterCasePermutation(String s) {
        helper(s, 0, "");
        return ans;
    }

    public void helper(String s, int curr, String temp) {
        if (curr == s.length()) {
            ans.add(temp);
            return;
        }
        if (Character.isDigit(s.charAt(curr))) {
            helper(s, curr + 1, temp + s.charAt(curr));
        } else {
            if (Character.isLowerCase(s.charAt(curr))) {
                helper(s, curr + 1, temp + s.charAt(curr));
                helper(s, curr + 1, temp + (s.substring(curr, curr + 1)).toUpperCase());
            } else {
                helper(s, curr + 1, temp + s.charAt(curr));
                helper(s, curr + 1, temp + (s.substring(curr, curr + 1)).toLowerCase());
            }
        }
    }
}
```