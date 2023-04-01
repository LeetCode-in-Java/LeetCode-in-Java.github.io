[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 556\. Next Greater Element III

Medium

Given a positive integer `n`, find _the smallest integer which has exactly the same digits existing in the integer_ `n` _and is greater in value than_ `n`. If no such positive integer exists, return `-1`.

**Note** that the returned integer should fit in **32-bit integer**, if there is a valid answer but it does not fit in **32-bit integer**, return `-1`.

**Example 1:**

**Input:** n = 12

**Output:** 21 

**Example 2:**

**Input:** n = 21

**Output:** -1 

**Constraints:**

*   <code>1 <= n <= 2<sup>31</sup> - 1</code>

## Solution

```java
public class Solution {
    /*
    - What this problem wants is finding the next permutation of n
    - Steps to find the next permuation:
       find largest index k such that inp[k] < inp[k+1];
           if k == -1: return -1
           else:
               look for largest index l such that inp[l] > inp[k]
               swap the two index
               reverse from k+1 to n.length
       */
    public int nextGreaterElement(int n) {
        char[] inp = String.valueOf(n).toCharArray();
        // Find k
        int k = -1;
        for (int i = inp.length - 2; i >= 0; i--) {
            if (inp[i] < inp[i + 1]) {
                k = i;
                break;
            }
        }
        if (k == -1) {
            return -1;
        }
        // Find l
        int largerIdx = inp.length - 1;
        for (int i = inp.length - 1; i >= 0; i--) {
            if (inp[i] > inp[k]) {
                largerIdx = i;
                break;
            }
        }
        swap(inp, k, largerIdx);
        reverse(inp, k + 1, inp.length - 1);
        // Build result
        int ret = 0;
        for (char c : inp) {
            int digit = c - '0';
            // Handle the case if ret > Integer.MAX_VALUE - This idea is borrowed from problem  8.
            // String to Integer (atoi)
            if (ret > Integer.MAX_VALUE / 10
                    || (ret == Integer.MAX_VALUE / 10 && digit > Integer.MAX_VALUE % 10)) {
                return -1;
            }
            ret = ret * 10 + (c - '0');
        }
        return ret;
    }

    private void swap(char[] inp, int i, int j) {
        char temp = inp[i];
        inp[i] = inp[j];
        inp[j] = temp;
    }

    private void reverse(char[] inp, int start, int end) {
        while (start < end) {
            char temp = inp[start];
            inp[start] = inp[end];
            inp[end] = temp;
            start++;
            end--;
        }
    }
}
```