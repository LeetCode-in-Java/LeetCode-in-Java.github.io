## 290\. Word Pattern

Easy

Given a `pattern` and a string `s`, find if `s` follows the same pattern.

Here **follow** means a full match, such that there is a bijection between a letter in `pattern` and a **non-empty** word in `s`.

**Example 1:**

**Input:** pattern = "abba", s = "dog cat cat dog"

**Output:** true 

**Example 2:**

**Input:** pattern = "abba", s = "dog cat cat fish"

**Output:** false 

**Example 3:**

**Input:** pattern = "aaaa", s = "dog cat cat dog"

**Output:** false 

**Example 4:**

**Input:** pattern = "abba", s = "dog dog dog dog"

**Output:** false 

**Constraints:**

*   `1 <= pattern.length <= 300`
*   `pattern` contains only lower-case English letters.
*   `1 <= s.length <= 3000`
*   `s` contains only lower-case English letters and spaces `' '`.
*   `s` **does not contain** any leading or trailing spaces.
*   All the words in `s` are separated by a **single space**.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public boolean wordPattern(String pattern, String s) {
        Map<Character, String> map = new HashMap<>();
        String[] words = s.split(" ");
        if (words.length != pattern.length()) {
            return false;
        }
        for (int i = 0; i < pattern.length(); i++) {
            if (!map.containsKey(pattern.charAt(i))) {
                if (map.containsValue(words[i])) {
                    return false;
                }
                map.put(pattern.charAt(i), words[i]);
            } else {
                if (!words[i].equals(map.get(pattern.charAt(i)))) {
                    return false;
                }
            }
        }
        return true;
    }
}
```