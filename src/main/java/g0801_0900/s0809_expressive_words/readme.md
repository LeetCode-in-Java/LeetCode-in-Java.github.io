[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 809\. Expressive Words

Medium

Sometimes people repeat letters to represent extra feeling. For example:

*   `"hello" -> "heeellooo"`
*   `"hi" -> "hiiii"`

In these strings like `"heeellooo"`, we have groups of adjacent letters that are all the same: `"h"`, `"eee"`, `"ll"`, `"ooo"`.

You are given a string `s` and an array of query strings `words`. A query word is **stretchy** if it can be made to be equal to `s` by any number of applications of the following extension operation: choose a group consisting of characters `c`, and add some number of characters `c` to the group so that the size of the group is **three or more**.

*   For example, starting with `"hello"`, we could do an extension on the group `"o"` to get `"hellooo"`, but we cannot get `"helloo"` since the group `"oo"` has a size less than three. Also, we could do another extension like `"ll" -> "lllll"` to get `"helllllooo"`. If `s = "helllllooo"`, then the query word `"hello"` would be **stretchy** because of these two extension operations: `query = "hello" -> "hellooo" -> "helllllooo" = s`.

Return _the number of query strings that are **stretchy**_.

**Example 1:**

**Input:** s = "heeellooo", words = ["hello", "hi", "helo"]

**Output:** 1

**Explanation:** 

We can extend "e" and "o" in the word "hello" to get "heeellooo". 

We can't extend "helo" to get "heeellooo" because the group "ll" is not size 3 or more.

**Example 2:**

**Input:** s = "zzzzzyyyyy", words = ["zzyy","zy","zyy"]

**Output:** 3

**Constraints:**

*   `1 <= s.length, words.length <= 100`
*   `1 <= words[i].length <= 100`
*   `s` and `words[i]` consist of lowercase letters.

## Solution

```java
public class Solution {
    public int expressiveWords(String s, String[] words) {
        int ans = 0;
        for (String w : words) {
            if (check(s, w)) {
                ans++;
            }
        }
        return ans;
    }

    private boolean check(String s, String w) {
        int i = 0;
        int j = 0;
        /* Logic is to check whether character at same index of S and w are same
          if same,
           1. Find the consecutive number of occurrences of the char in S (say len1) and w ( say len2)
           2. If len1 == len 2 , move to the next char in S and w
           3. If  len1 >= 3 and len2 < len1, means we can make the char in w stretchy to match len1
           4. else, return false, because it's not possible to stretch the char in w
        */
        while (i < s.length() && j < w.length()) {
            char ch1 = s.charAt(i);
            char ch2 = w.charAt(j);

            int len1 = getLen(s, i);
            int len2 = getLen(w, j);
            if (ch1 == ch2) {
                if (len1 == len2 || (len1 >= 3 && len2 < len1)) {
                    i = i + len1;
                    j = j + len2;
                } else {
                    return false;
                }
            } else {
                return false;
            }
        }
        return i == s.length() && j == w.length();
    }

    private int getLen(String value, int i) {
        i = i + 1;
        int count = 1;
        for (int j = i; j < value.length(); j++) {
            if (value.charAt(j) == value.charAt(i - 1)) {
                count++;
            } else {
                break;
            }
        }
        return count;
    }
}
```