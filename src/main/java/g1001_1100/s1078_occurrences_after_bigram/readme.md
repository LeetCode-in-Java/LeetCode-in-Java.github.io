[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1078\. Occurrences After Bigram

Easy

Given two strings `first` and `second`, consider occurrences in some text of the form `"first second third"`, where `second` comes immediately after `first`, and `third` comes immediately after `second`.

Return _an array of all the words_ `third` _for each occurrence of_ `"first second third"`.

**Example 1:**

**Input:** text = "alice is a good girl she is a good student", first = "a", second = "good"

**Output:** ["girl","student"]

**Example 2:**

**Input:** text = "we will we will rock you", first = "we", second = "will"

**Output:** ["we","rock"]

**Constraints:**

*   `1 <= text.length <= 1000`
*   `text` consists of lowercase English letters and spaces.
*   All the words in `text` a separated by **a single space**.
*   `1 <= first.length, second.length <= 10`
*   `first` and `second` consist of lowercase English letters.

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public String[] findOcurrences(String text, String first, String second) {
        List<String> list = new ArrayList<>();
        String[] str = text.split(" ");
        for (int i = 0; i < str.length; i++) {
            if (str[i].equals(first) && str.length - 1 >= i + 2 && str[i + 1].equals(second)) {
                list.add(str[i + 2]);
            }
        }
        String[] s = new String[list.size()];
        int j = 0;
        for (String ele : list) {
            s[j++] = ele;
        }
        return s;
    }
}
```