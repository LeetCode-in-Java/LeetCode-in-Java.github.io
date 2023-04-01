[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1419\. Minimum Number of Frogs Croaking

Medium

You are given the string `croakOfFrogs`, which represents a combination of the string `"croak"` from different frogs, that is, multiple frogs can croak at the same time, so multiple `"croak"` are mixed.

_Return the minimum number of_ different _frogs to finish all the croaks in the given string._

A valid `"croak"` means a frog is printing five letters `'c'`, `'r'`, `'o'`, `'a'`, and `'k'` **sequentially**. The frogs have to print all five letters to finish a croak. If the given string is not a combination of a valid `"croak"` return `-1`.

**Example 1:**

**Input:** croakOfFrogs = "croakcroak"

**Output:** 1

**Explanation:** One frog yelling "croak**"** twice.

**Example 2:**

**Input:** croakOfFrogs = "crcoakroak"

**Output:** 2

**Explanation:** The minimum number of frogs is two. The first frog could yell "**cr**c**oak**roak". The second frog could yell later "cr**c**oak**roak**".

**Example 3:**

**Input:** croakOfFrogs = "croakcrook"

**Output:** -1

**Explanation:** The given string is an invalid combination of "croak**"** from different frogs.

**Constraints:**

*   <code>1 <= croakOfFrogs.length <= 10<sup>5</sup></code>
*   `croakOfFrogs` is either `'c'`, `'r'`, `'o'`, `'a'`, or `'k'`.

## Solution

```java
public class Solution {
    public int minNumberOfFrogs(String s) {
        int ans = 0;
        int[] f = new int[26];
        for (int i = 0; i < s.length(); i++) {
            f[s.charAt(i) - 'a']++;
            if (s.charAt(i) == 'k' && checkEnough(f)) {
                reduce(f);
            }
            if (!isValid(f)) {
                return -1;
            }
            ans = Math.max(ans, getMax(f));
        }
        return isEmpty(f) ? ans : -1;
    }

    private boolean checkEnough(int[] f) {
        return f['c' - 'a'] > 0
                && f['r' - 'a'] > 0
                && f['o' - 'a'] > 0
                && f[0] > 0
                && f['k' - 'a'] > 0;
    }

    public void reduce(int[] f) {

        f['c' - 'a']--;
        f['r' - 'a']--;
        f['o' - 'a']--;
        f[0]--;
        f['k' - 'a']--;
    }

    private int getMax(int[] f) {
        int max = 0;
        for (int v : f) {
            max = Math.max(max, v);
        }
        return max;
    }

    private boolean isEmpty(int[] f) {
        for (int v : f) {
            if (v > 0) {
                return false;
            }
        }
        return true;
    }

    private boolean isValid(int[] f) {
        if (f['r' - 'a'] > f['c' - 'a']) {
            return false;
        }
        if (f['o' - 'a'] > f['r' - 'a']) {
            return false;
        }
        if (f[0] > f['o' - 'a']) {
            return false;
        }
        return f['k' - 'a'] <= f[0];
    }
}
```