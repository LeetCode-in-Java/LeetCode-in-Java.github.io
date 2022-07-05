[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 500\. Keyboard Row

Easy

Given an array of strings `words`, return _the words that can be typed using letters of the alphabet on only one row of American keyboard like the image below_.

In the **American keyboard**:

*   the first row consists of the characters `"qwertyuiop"`,
*   the second row consists of the characters `"asdfghjkl"`, and
*   the third row consists of the characters `"zxcvbnm"`.

![](https://assets.leetcode.com/uploads/2018/10/12/keyboard.png)

**Example 1:**

**Input:** words = ["Hello","Alaska","Dad","Peace"]

**Output:** ["Alaska","Dad"]

**Example 2:**

**Input:** words = ["omk"]

**Output:** []

**Example 3:**

**Input:** words = ["adsdf","sfd"]

**Output:** ["adsdf","sfd"]

**Constraints:**

*   `1 <= words.length <= 20`
*   `1 <= words[i].length <= 100`
*   `words[i]` consists of English letters (both lowercase and uppercase).

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    private boolean check(String str, String word) {
        for (char ch : word.toCharArray()) {
            if (str.indexOf(ch) < 0) {
                return false;
            }
        }
        return true;
    }

    public String[] findWords(String[] words) {
        List<String> arr = new ArrayList<>();
        for (String word : words) {
            String w = word.toLowerCase();
            if (check("qwertyuiop", w) || check("asdfghjkl", w) || check("zxcvbnm", w)) {
                arr.add(word);
            }
        }
        String[] ans = new String[arr.size()];
        ans = arr.toArray(ans);
        return ans;
    }
}
```