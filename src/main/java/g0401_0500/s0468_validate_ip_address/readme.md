[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 468\. Validate IP Address

Medium

Given a string `queryIP`, return `"IPv4"` if IP is a valid IPv4 address, `"IPv6"` if IP is a valid IPv6 address or `"Neither"` if IP is not a correct IP of any type.

**A valid IPv4** address is an IP in the form <code>"x<sub>1</sub>.x<sub>2</sub>.x<sub>3</sub>.x<sub>4</sub>"</code> where <code>0 <= x<sub>i</sub> <= 255</code> and <code>x<sub>i</sub></code> **cannot contain** leading zeros. For example, `"192.168.1.1"` and `"192.168.1.0"` are valid IPv4 addresses but `"192.168.01.1"`, while `"192.168.1.00"` and `"192.168@1.1"` are invalid IPv4 addresses.

**A valid IPv6** address is an IP in the form <code>"x<sub>1</sub>:x<sub>2</sub>:x<sub>3</sub>:x<sub>4</sub>:x<sub>5</sub>:x<sub>6</sub>:x<sub>7</sub>:x<sub>8</sub>"</code> where:

*   <code>1 <= x<sub>i</sub>.length <= 4</code>
*   <code>x<sub>i</sub></code> is a **hexadecimal string** which may contain digits, lower-case English letter (`'a'` to `'f'`) and upper-case English letters (`'A'` to `'F'`).
*   Leading zeros are allowed in <code>x<sub>i</sub></code>.

For example, "`2001:0db8:85a3:0000:0000:8a2e:0370:7334"` and "`2001:db8:85a3:0:0:8A2E:0370:7334"` are valid IPv6 addresses, while "`2001:0db8:85a3::8A2E:037j:7334"` and "`02001:0db8:85a3:0000:0000:8a2e:0370:7334"` are invalid IPv6 addresses.

**Example 1:**

**Input:** queryIP = "172.16.254.1"

**Output:** "IPv4"

**Explanation:** This is a valid IPv4 address, return "IPv4".

**Example 2:**

**Input:** queryIP = "2001:0db8:85a3:0:0:8A2E:0370:7334"

**Output:** "IPv6"

**Explanation:** This is a valid IPv6 address, return "IPv6".

**Example 3:**

**Input:** queryIP = "256.256.256.256"

**Output:** "Neither"

**Explanation:** This is neither a IPv4 address nor a IPv6 address.

**Constraints:**

*   `queryIP` consists only of English letters, digits and the characters `'.'` and `':'`.

## Solution

```java
public class Solution {
    private static final String NEITHER = "Neither";

    public String validIPAddress(String ip) {
        if (ip.length() == 0) {
            return NEITHER;
        }
        String[] arr = ip.split("\\.", -1);
        String[] arr1 = ip.split(":", -1);
        if (arr.length == 4) {
            for (String num : arr) {
                try {
                    if ((num.length() > 1 && num.startsWith("0")) || Integer.parseInt(num) > 255) {
                        return NEITHER;
                    }
                } catch (Exception e) {
                    return NEITHER;
                }
            }
            return "IPv4";
        } else if (arr1.length == 8) {
            for (String num : arr1) {
                if (num.length() < 1 || num.length() > 4) {
                    return NEITHER;
                }
                for (int j = 0; j < num.length(); j++) {
                    char ch = num.charAt(j);
                    if (ch > 9
                            && (Character.isLowerCase(ch) && ch > 'f'
                                    || Character.isUpperCase(ch) && ch > 'F')) {
                        return NEITHER;
                    }
                }
            }
            return "IPv6";
        }
        return NEITHER;
    }
}
```