[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 564\. Find the Closest Palindrome

Hard

Given a string `n` representing an integer, return _the closest integer (not including itself), which is a palindrome_. If there is a tie, return _**the smaller one**_.

The closest is defined as the absolute difference minimized between two integers.

**Example 1:**

**Input:** n = "123"

**Output:** "121"

**Example 2:**

**Input:** n = "1"

**Output:** "0"

**Explanation:** 0 and 2 are the closest palindromes but we return the smallest which is 0.

**Constraints:**

*   `1 <= n.length <= 18`
*   `n` consists of only digits.
*   `n` does not have leading zeros.
*   `n` is representing an integer in the range <code>[1, 10<sup>18</sup> - 1]</code>.

## Solution

```java
@SuppressWarnings("java:S2184")
public class Solution {
    public String nearestPalindromic(String n) {
        if (n.length() == 1) {
            return String.valueOf(Integer.parseInt(n) - 1);
        }
        long num = Long.parseLong(n);
        int offset = (int) Math.pow(10, n.length() / 2);
        long first =
                isPalindrome(n)
                        ? palindromeGenerator(num + offset, n.length())
                        : palindromeGenerator(num, n.length());
        long second =
                first < num
                        ? palindromeGenerator(num + offset, n.length())
                        : palindromeGenerator(num - offset, n.length());

        if (first + second == 2 * num) {
            return first < second ? String.valueOf(first) : String.valueOf(second);
        }

        return Math.abs(num - first) > Math.abs(num - second)
                ? String.valueOf(second)
                : String.valueOf(first);
    }

    private long palindromeGenerator(long num, int length) {
        if (num < 10) {
            return 9;
        }
        int numOfDigits = String.valueOf(num).length();
        if (numOfDigits > length) {
            return ((long) Math.pow(10, numOfDigits - 1) + 1);
        } else if (numOfDigits < length) {
            return ((long) Math.pow(10, numOfDigits) - 1);
        }

        num = num - num % (long) Math.pow(10, numOfDigits / 2);
        long temp = num;
        for (int j = 0; j < numOfDigits / 2; j++) {
            long digit = (long) Math.pow(10, numOfDigits - j - 1);
            num += (int) ((temp / digit) * Math.pow(10, j));
            temp = temp % digit;
        }
        return num;
    }

    private boolean isPalindrome(String str) {
        for (int i = 0; i < str.length() / 2; i++) {
            if (str.charAt(i) != str.charAt(str.length() - 1 - i)) {
                return false;
            }
        }
        return true;
    }
}
```