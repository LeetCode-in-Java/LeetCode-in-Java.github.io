[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 420\. Strong Password Checker

Hard

A password is considered strong if the below conditions are all met:

*   It has at least `6` characters and at most `20` characters.
*   It contains at least **one lowercase** letter, at least **one uppercase** letter, and at least **one digit**.
*   It does not contain three repeating characters in a row (i.e., `"...aaa..."` is weak, but `"...aa...a..."` is strong, assuming other conditions are met).

Given a string `password`, return _the minimum number of steps required to make `password` strong. if `password` is already strong, return `0`._

In one step, you can:

*   Insert one character to `password`,
*   Delete one character from `password`, or
*   Replace one character of `password` with another character.

**Example 1:**

**Input:** password = "a"

**Output:** 5 

**Example 2:**

**Input:** password = "aA1"

**Output:** 3 

**Example 3:**

**Input:** password = "1337C0d3"

**Output:** 0 

**Constraints:**

*   `1 <= password.length <= 50`
*   `password` consists of letters, digits, dot `'.'` or exclamation mark `'!'`.

## Solution

```java
public class Solution {
    public int strongPasswordChecker(String s) {
        int res = 0;
        int a1 = 1;
        int a2 = 1;
        int d = 1;
        char[] carr = s.toCharArray();
        int[] arr = new int[carr.length];
        int i1 = 0;
        while (i1 < arr.length) {
            if (Character.isLowerCase(carr[i1])) {
                a1 = 0;
            }
            if (Character.isUpperCase(carr[i1])) {
                a2 = 0;
            }
            if (Character.isDigit(carr[i1])) {
                d = 0;
            }
            int j = i1;
            while (i1 < carr.length && carr[i1] == carr[j]) {
                i1++;
            }
            arr[j] = i1 - j;
        }
        int totalMissing = (a1 + a2 + d);
        if (arr.length < 6) {
            res += totalMissing + Math.max(0, 6 - (arr.length + totalMissing));
        } else {
            int overLen = Math.max(arr.length - 20, 0);
            int leftOver = 0;
            res += overLen;
            for (int k = 1; k < 3; k++) {
                for (int i = 0; i < arr.length && overLen > 0; i++) {
                    if (arr[i] < 3 || arr[i] % 3 != (k - 1)) {
                        continue;
                    }
                    arr[i] -= Math.min(overLen, k);
                    overLen -= k;
                }
            }
            for (int i = 0; i < arr.length; i++) {
                if (arr[i] >= 3 && overLen > 0) {
                    int need = arr[i] - 2;
                    arr[i] -= overLen;
                    overLen -= need;
                }
                if (arr[i] >= 3) {
                    leftOver += arr[i] / 3;
                }
            }
            res += Math.max(totalMissing, leftOver);
        }
        return res;
    }
}
```