[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 125\. Valid Palindrome

Easy

A phrase is a **palindrome** if, after converting all uppercase letters into lowercase letters and removing all non-alphanumeric characters, it reads the same forward and backward. Alphanumeric characters include letters and numbers.

Given a string `s`, return `true` _if it is a **palindrome**, or_ `false` _otherwise_.

**Example 1:**

**Input:** s = "A man, a plan, a canal: Panama"

**Output:** true

**Explanation:** "amanaplanacanalpanama" is a palindrome. 

**Example 2:**

**Input:** s = "race a car"

**Output:** false

**Explanation:** "raceacar" is not a palindrome. 

**Example 3:**

**Input:** s = " "

**Output:** true

**Explanation:** s is an empty string "" after removing non-alphanumeric characters. Since an empty string reads the same forward and backward, it is a palindrome. 

**Constraints:**

*   <code>1 <= s.length <= 2 * 10<sup>5</sup></code>
*   `s` consists only of printable ASCII characters.

## Solution

```java
public class Solution {
    public boolean isPalindrome(String s) {
        int i = 0;
        int j = s.length() - 1;
        boolean res = true;
        while (res) {
            // Iterates through string to find first char which is alphanumeric.
            // Done to ignore non-alphanumeric characters.
            // Starts from 0 to j-1.
            while (i < j && isNotAlphaNumeric(s.charAt(i))) {
                i++;
            }
            // Similarly from j-1 to 0.
            while (i < j && isNotAlphaNumeric(s.charAt(j))) {
                j--;
            }
            // Checks if i is greater than or equal to j.
            // The main loop only needs to loop n / 2 times hence this condition (where n is string
            // length).
            if (i >= j) {
                break;
            }
            // Assigning found indices to variables.
            // The upperToLower function is used to convert characters, if upper case, to lower
            // case.
            // If already lower case, it'll return as it is.
            char left = upperToLower(s.charAt(i));
            char right = upperToLower(s.charAt(j));
            // If both variables are not same, result becomes false, and breaks out of the loop at
            // next iteration.
            if (left != right) {
                res = false;
            }
            i++;
            j--;
        }
        return res;
    }

    private boolean isNotAlphaNumeric(char c) {
        return (c < 'a' || c > 'z') && (c < 'A' || c > 'Z') && (c < '0' || c > '9');
    }

    private boolean isUpper(char c) {
        return c >= 'A' && c <= 'Z';
    }

    private char upperToLower(char c) {
        if (isUpper(c)) {
            c = (char) (c + 32);
        }
        return c;
    }
}
```