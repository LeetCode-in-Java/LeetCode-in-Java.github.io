## 709\. To Lower Case

Easy

Given a string `s`, return _the string after replacing every uppercase letter with the same lowercase letter_.

**Example 1:**

**Input:** s = "Hello"

**Output:** "hello"

**Example 2:**

**Input:** s = "here"

**Output:** "here"

**Example 3:**

**Input:** s = "LOVELY"

**Output:** "lovely"

**Constraints:**

*   `1 <= s.length <= 100`
*   `s` consists of printable ASCII characters.

## Solution

```java
public class Solution {
    public String toLowerCase(String s) {
        char[] c = s.toCharArray();
        for (int i = 0; i < s.length(); i++) {
            if (c[i] <= 'Z' && c[i] >= 'A') {
                c[i] = (char) (c[i] - 'A' + 'a');
            }
        }
        return new String(c);
    }
}
```