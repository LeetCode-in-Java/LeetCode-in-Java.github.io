[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1616\. Split Two Strings to Make Palindrome

Medium

You are given two strings `a` and `b` of the same length. Choose an index and split both strings **at the same index**, splitting `a` into two strings: <code>a<sub>prefix</sub></code> and <code>a<sub>suffix</sub></code> where <code>a = a<sub>prefix</sub> + a<sub>suffix</sub></code>, and splitting `b` into two strings: <code>b<sub>prefix</sub></code> and <code>b<sub>suffix</sub></code> where <code>b = b<sub>prefix</sub> + b<sub>suffix</sub></code>. Check if <code>a<sub>prefix</sub> + b<sub>suffix</sub></code> or <code>b<sub>prefix</sub> + a<sub>suffix</sub></code> forms a palindrome.

When you split a string `s` into <code>s<sub>prefix</sub></code> and <code>s<sub>suffix</sub></code>, either <code>s<sub>suffix</sub></code> or <code>s<sub>prefix</sub></code> is allowed to be empty. For example, if `s = "abc"`, then `"" + "abc"`, `"a" + "bc"`, `"ab" + "c"` , and `"abc" + ""` are valid splits.

Return `true` _if it is possible to form_ _a palindrome string, otherwise return_ `false`.

**Notice** that `x + y` denotes the concatenation of strings `x` and `y`.

**Example 1:**

**Input:** a = "x", b = "y"

**Output:** true **Explaination:** If either a or b are palindromes the answer is true since you can split in the following way: 

a<sub>prefix</sub> = "", a<sub>suffix</sub> = "x" 

b<sub>prefix</sub> = "", b<sub>suffix</sub> = "y" 

Then, a<sub>prefix</sub> + b<sub>suffix</sub> = "" + "y" = "y", which is a palindrome.

**Example 2:**

**Input:** a = "xbdef", b = "xecab"

**Output:** false

**Example 3:**

**Input:** a = "ulacfd", b = "jizalu"

**Output:** true **Explaination:** Split them at index 3: 

a<sub>prefix</sub> = "ula", a<sub>suffix</sub> = "cfd" 

b<sub>prefix</sub> = "jiz", b<sub>suffix</sub> = "alu" 

Then, a<sub>prefix</sub> + b<sub>suffix</sub> = "ula" + "alu" = "ulaalu", which is a palindrome.

**Constraints:**

*   <code>1 <= a.length, b.length <= 10<sup>5</sup></code>
*   `a.length == b.length`
*   `a` and `b` consist of lowercase English letters

## Solution

```java
@SuppressWarnings("java:S2234")
public class Solution {
    public boolean checkPalindromeFormation(String a, String b) {
        int n = a.length();
        int s = 0;
        int e = n - 1;
        if (isPalindrome(a, b, s, e, true)) {
            return true;
        } else {
            return isPalindrome(b, a, s, e, true);
        }
    }

    private boolean isPalindrome(String a, String b, int s, int e, boolean check) {
        if (s == e) {
            return true;
        }
        while (s < e) {
            if (a.charAt(s) != b.charAt(e)) {
                return check
                        && (isPalindrome(a, a, s, e, false) || isPalindrome(b, b, s, e, false));
            }
            s++;
            e--;
        }
        return true;
    }
}
```