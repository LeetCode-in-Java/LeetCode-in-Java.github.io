[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2266\. Count Number of Texts

Medium

Alice is texting Bob using her phone. The **mapping** of digits to letters is shown in the figure below.

![](https://assets.leetcode.com/uploads/2022/03/15/1200px-telephone-keypad2svg.png)

In order to **add** a letter, Alice has to **press** the key of the corresponding digit `i` times, where `i` is the position of the letter in the key.

*   For example, to add the letter `'s'`, Alice has to press `'7'` four times. Similarly, to add the letter `'k'`, Alice has to press `'5'` twice.
*   Note that the digits `'0'` and `'1'` do not map to any letters, so Alice **does not** use them.

However, due to an error in transmission, Bob did not receive Alice's text message but received a **string of pressed keys** instead.

*   For example, when Alice sent the message `"bob"`, Bob received the string `"2266622"`.

Given a string `pressedKeys` representing the string received by Bob, return _the **total number of possible text messages** Alice could have sent_.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

**Example 1:**

**Input:** pressedKeys = "22233"

**Output:** 8

**Explanation:** 

The possible text messages Alice could have sent are: 

"aaadd", "abdd", "badd", "cdd", "aaae", "abe", "bae", and "ce". 

Since there are 8 possible messages, we return 8.

**Example 2:**

**Input:** pressedKeys = "222222222222222222222222222222222222"

**Output:** 82876089

**Explanation:** There are 2082876103 possible text messages Alice could have sent. 

Since we need to return the answer modulo 10<sup>9</sup> + 7, we return 2082876103 % (10<sup>9</sup> + 7) = 82876089.

**Constraints:**

*   <code>1 <= pressedKeys.length <= 10<sup>5</sup></code>
*   `pressedKeys` only consists of digits from `'2'` - `'9'`.

## Solution

```java
@SuppressWarnings("java:S135")
public class Solution {
    public int countTexts(String pressedKeys) {
        int len = pressedKeys.length();
        long dp0 = 1L;
        long dp1 = 0;
        long dp2 = 0;
        long dp3 = 0;
        long dp4;
        char[] keys = pressedKeys.toCharArray();
        int base = 1000000007;
        for (int i = 1; i < len; i++) {
            int r = keys[i] - '0';
            dp4 = dp3;
            dp3 = dp2;
            dp2 = dp1;
            dp1 = dp0 % base;
            dp0 = dp1;
            dp0 += (i - 1 == 0 && keys[i] == keys[i - 1]) ? 1 : 0;
            if (i - 1 <= 0 || keys[i] != keys[i - 1]) {
                continue;
            }
            dp0 += dp2;
            dp0 += (i - 2 == 0 && keys[i] == keys[i - 2]) ? 1 : 0;
            if (i - 2 <= 0 || keys[i] != keys[i - 2]) {
                continue;
            }
            dp0 += dp3;
            dp0 += (i - 3 == 0 && keys[i] == keys[i - 3] && (r == 7 || r == 9)) ? 1 : 0;
            if (i - 3 <= 0 || keys[i] != keys[i - 3] || (r != 7 && r != 9)) {
                continue;
            }
            dp0 += dp4;
        }

        return (int) (dp0 % base);
    }
}
```