[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1410\. HTML Entity Parser

Medium

**HTML entity parser** is the parser that takes HTML code as input and replace all the entities of the special characters by the characters itself.

The special characters and their entities for HTML are:

*   **Quotation Mark:** the entity is `&quot;` and symbol character is `"`.
*   **Single Quote Mark:** the entity is `&apos;` and symbol character is `'`.
*   **Ampersand:** the entity is `&amp;` and symbol character is `&`.
*   **Greater Than Sign:** the entity is `&gt;` and symbol character is `>`.
*   **Less Than Sign:** the entity is `&lt;` and symbol character is `<`.
*   **Slash:** the entity is `&frasl;` and symbol character is `/`.

Given the input `text` string to the HTML parser, you have to implement the entity parser.

Return _the text after replacing the entities by the special characters_.

**Example 1:**

**Input:** text = "&amp; is an HTML entity but &ambassador; is not."

**Output:** "& is an HTML entity but &ambassador; is not."

**Explanation:** The parser will replace the &amp; entity by &

**Example 2:**

**Input:** text = "and I quote: &quot;...&quot;"

**Output:** "and I quote: \\"...\\""

**Constraints:**

*   <code>1 <= text.length <= 10<sup>5</sup></code>
*   The string may contain any possible characters out of all the 256 ASCII characters.

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public String entityParser(String text) {
        Map<String, String> map = new HashMap<>();
        map.put("&quot;", "\"");
        map.put("&apos;", "'");
        map.put("&amp;", "&");
        map.put("&gt;", ">");
        map.put("&lt;", "<");
        map.put("&frasl;", "/");
        int n = text.length();
        StringBuilder sb = new StringBuilder();
        int i = 0;
        while (i < n) {
            char c = text.charAt(i);
            if (c == '&') {
                int index = text.indexOf(";", i);
                if (index >= 0) {
                    String pattern = text.substring(i, index + 1);
                    if (map.containsKey(pattern)) {
                        sb.append(map.get(pattern));
                        i += pattern.length();
                        continue;
                    }
                }
            }
            sb.append(c);
            i++;
        }
        return sb.toString();
    }
}
```