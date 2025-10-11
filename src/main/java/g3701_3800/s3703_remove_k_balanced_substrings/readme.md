[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3703\. Remove K-Balanced Substrings

Medium

You are given a string `s` consisting of `'('` and `')'`, and an integer `k`.

A **string** is **k-balanced** if it is **exactly** `k` **consecutive** `'('` followed by `k` **consecutive** `')'`, i.e., `'(' * k + ')' * k`.

For example, if `k = 3`, k-balanced is `"((()))"`.

You must **repeatedly** remove all **non-overlapping k-balanced **substring**** from `s`, and then join the remaining parts. Continue this process until no k-balanced **substring** exists.

Return the final string after all possible removals.

**Example 1:**

**Input:** s = "(())", k = 1

**Output:** ""

**Explanation:**

k-balanced substring is `"()"`

| Step | Current `s`  | `k-balanced`             | Result `s` |
|------|--------------|--------------------------|------------|
| 1    | `(() )`      | `(<s>**()**</s>)`        | `()`       |
| 2    | `()`         | `<s>**()`**</s>`         | Empty      |

Thus, the final string is `""`.

**Example 2:**

**Input:** s = "(()(", k = 1

**Output:** "(("

**Explanation:**

k-balanced substring is `"()"`

| Step | Current `s`  | `k-balanced`         | Result `s` |
|------|--------------|----------------------|------------|
| 1    | `(()(`       | `(~**()**~)(`        | `((`       |
| 2    | `((`         | -                    | `((`       |

Thus, the final string is `"(("`.

**Example 3:**

**Input:** s = "((()))()()()", k = 3

**Output:** "()()()"

**Explanation:**

k-balanced substring is `"((()))"`

| Step | Current `s`       | `k-balanced`                     | Result `s` |
|------|-------------------|----------------------------------|------------|
| 1    | `((()))()()()`    | ~~**((()))**~~`()()()`           | `()()()`   |
| 2    | `()()()`          | -                                | `()()()`   |

Thus, the final string is `"()()()"`.

**Constraints:**

*   <code>2 <= s.length <= 10<sup>5</sup></code>
*   `s` consists only of `'('` and `')'`.
*   `1 <= k <= s.length / 2`

## Solution

```java
public class Solution {
    public String removeSubstring(String s, int k) {
        StringBuilder sb = new StringBuilder();
        int count = 0;
        for (char ch : s.toCharArray()) {
            sb.append(ch);
            if (ch == '(') {
                count++;
            } else {
                if (count >= k && sb.length() >= 2 * k) {
                    int len = sb.length();
                    boolean b = true;
                    for (int i = len - 2 * k; i < len - k; i++) {
                        if (sb.charAt(i) != '(') {
                            b = false;
                            break;
                        }
                    }
                    for (int i = len - k; i < len; i++) {
                        if (sb.charAt(i) != ')') {
                            b = false;
                            break;
                        }
                    }
                    if (b) {
                        sb.delete(sb.length() - 2 * k, sb.length());
                        count -= k;
                    }
                }
            }
        }
        return sb.toString();
    }
}
```