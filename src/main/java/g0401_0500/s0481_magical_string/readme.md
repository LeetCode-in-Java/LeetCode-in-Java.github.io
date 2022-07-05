[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 481\. Magical String

Medium

A magical string `s` consists of only `'1'` and `'2'` and obeys the following rules:

*   The string s is magical because concatenating the number of contiguous occurrences of characters `'1'` and `'2'` generates the string `s` itself.

The first few elements of `s` is `s = "1221121221221121122……"`. If we group the consecutive `1`'s and `2`'s in `s`, it will be `"1 22 11 2 1 22 1 22 11 2 11 22 ......"` and the occurrences of `1`'s or `2`'s in each group are `"1 2 2 1 1 2 1 2 2 1 2 2 ......"`. You can see that the occurrence sequence is `s` itself.

Given an integer `n`, return the number of `1`'s in the first `n` number in the magical string `s`.

**Example 1:**

**Input:** n = 6

**Output:** 3

**Explanation:** The first 6 elements of magical string s is "122112" and it contains three 1's, so return 3.

**Example 2:**

**Input:** n = 1

**Output:** 1

**Constraints:**

*   <code>1 <= n <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int magicalString(int n) {
        int[] a = new int[n + 2];
        int fast = 1;
        int slow = 1;
        int num = 1;
        while (fast <= n) {
            a[fast++] = num;
            if (a[slow++] == 2) {
                a[fast++] = num;
            }
            num = 3 - num;
        }
        int count = 0;
        for (int j = 1; j <= n; j++) {
            if (a[j] == 1) {
                count++;
            }
        }
        return count;
    }
}
```