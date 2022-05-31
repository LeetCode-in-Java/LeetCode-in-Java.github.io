## 557\. Reverse Words in a String III

Easy

Given a string `s`, reverse the order of characters in each word within a sentence while still preserving whitespace and initial word order.

**Example 1:**

**Input:** s = "Let's take LeetCode contest"

**Output:** "s'teL ekat edoCteeL tsetnoc" 

**Example 2:**

**Input:** s = "God Ding"

**Output:** "doG gniD" 

**Constraints:**

*   <code>1 <= s.length <= 5 * 10<sup>4</sup></code>
*   `s` contains printable **ASCII** characters.
*   `s` does not contain any leading or trailing spaces.
*   There is **at least one** word in `s`.
*   All the words in `s` are separated by a single space.

## Solution

```java
public class Solution {
    public String reverseWords(String s) {
        int l = 0;
        int r = 0;
        int len = s.length();
        char[] ch = s.toCharArray();
        for (int i = 0; i <= len; i++) {
            if (i == len || ch[i] == ' ') {
                l = r;
                r = i;
                reverse(ch, l, r - 1);
                r++;
            }
        }
        return new String(ch);
    }

    private void reverse(char[] s, int l, int r) {
        char c;
        while (r > l) {
            c = s[l];
            s[l++] = s[r];
            s[r--] = c;
        }
    }
}
```