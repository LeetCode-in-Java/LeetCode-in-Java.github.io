[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 929\. Unique Email Addresses

Easy

Every **valid email** consists of a **local name** and a **domain name**, separated by the `'@'` sign. Besides lowercase letters, the email may contain one or more `'.'` or `'+'`.

*   For example, in `"alice@leetcode.com"`, `"alice"` is the **local name**, and `"leetcode.com"` is the **domain name**.

If you add periods `'.'` between some characters in the **local name** part of an email address, mail sent there will be forwarded to the same address without dots in the local name. Note that this rule **does not apply** to **domain names**.

*   For example, `"alice.z@leetcode.com"` and `"alicez@leetcode.com"` forward to the same email address.

If you add a plus `'+'` in the **local name**, everything after the first plus sign **will be ignored**. This allows certain emails to be filtered. Note that this rule **does not apply** to **domain names**.

*   For example, `"m.y+name@email.com"` will be forwarded to `"my@email.com"`.

It is possible to use both of these rules at the same time.

Given an array of strings `emails` where we send one email to each `emails[i]`, return _the number of different addresses that actually receive mails_.

**Example 1:**

**Input:** emails = ["test.email+alex@leetcode.com","test.e.mail+bob.cathy@leetcode.com","testemail+david@lee.tcode.com"]

**Output:** 2

**Explanation:** "testemail@leetcode.com" and "testemail@lee.tcode.com" actually receive mails.

**Example 2:**

**Input:** emails = ["a@leetcode.com","b@leetcode.com","c@leetcode.com"]

**Output:** 3

**Constraints:**

*   `1 <= emails.length <= 100`
*   `1 <= emails[i].length <= 100`
*   `emails[i]` consist of lowercase English letters, `'+'`, `'.'` and `'@'`.
*   Each `emails[i]` contains exactly one `'@'` character.
*   All local and domain names are non-empty.
*   Local names do not start with a `'+'` character.
*   Domain names end with the `".com"` suffix.

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public int numUniqueEmails(String[] emails) {
        Set<String> set = new HashSet<>();
        for (String s : emails) {
            StringBuilder sb = new StringBuilder();
            int i = 0;
            while (i < s.length()) {
                char c = s.charAt(i);
                if (c == '+' || c == '@') {
                    sb.append('@');
                    i = s.indexOf("@") + 1;
                    sb.append(s.substring(i));
                    break;
                } else if (c != '.') {
                    sb.append(c);
                }
                i++;
            }
            set.add(sb.toString());
        }
        return set.size();
    }
}
```