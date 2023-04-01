[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 906\. Super Palindromes

Hard

Let's say a positive integer is a **super-palindrome** if it is a palindrome, and it is also the square of a palindrome.

Given two positive integers `left` and `right` represented as strings, return _the number of **super-palindromes** integers in the inclusive range_ `[left, right]`.

**Example 1:**

**Input:** left = "4", right = "1000"

**Output:** 4

**Explanation:** 4, 9, 121, and 484 are superpalindromes. Note that 676 is not a superpalindrome: 26 \* 26 = 676, but 26 is not a palindrome.

**Example 2:**

**Input:** left = "1", right = "2"

**Output:** 1

**Constraints:**

*   `1 <= left.length, right.length <= 18`
*   `left` and `right` consist of only digits.
*   `left` and `right` cannot have leading zeros.
*   `left` and `right` represent integers in the range <code>[1, 10<sup>18</sup> - 1]</code>.
*   `left` is less than or equal to `right`.

## Solution

```java
public class Solution {
    public int superpalindromesInRange(String left, String right) {
        long l = Long.parseLong(left);
        long r = Long.parseLong(right);
        int cnt = 0;
        long cur = 1;
        while (true) {
            long p1 = getPalindromeIncLastDigit(cur);
            long p2 = getPalindromeExcLastDigit(cur);
            long sq1 = p1 * p1;
            long sq2 = p2 * p2;
            if (sq2 > r) {
                break;
            }
            if (sq1 >= l && sq1 <= r && isPalindrome(sq1)) {
                cnt++;
            }
            if (sq2 >= l && isPalindrome(sq2)) {
                cnt++;
            }
            cur++;
        }
        return cnt;
    }

    private boolean isPalindrome(long val) {
        long construct = 0;
        if (val % 10 == 0 && val >= 10) {
            return false;
        }
        while (construct < val) {
            construct = construct * 10 + val % 10;
            val /= 10;
        }
        return construct == val || construct / 10 == val;
    }

    private long getPalindromeIncLastDigit(long val) {
        long copy = val;
        while (copy != 0) {
            val = val * 10 + copy % 10;
            copy /= 10;
        }
        return val;
    }

    private long getPalindromeExcLastDigit(long val) {
        long copy = val / 10;
        while (copy != 0) {
            val = val * 10 + copy % 10;
            copy /= 10;
        }
        return val;
    }
}
```