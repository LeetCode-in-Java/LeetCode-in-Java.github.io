[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2217\. Find Palindrome With Fixed Length

Medium

Given an integer array `queries` and a **positive** integer `intLength`, return _an array_ `answer` _where_ `answer[i]` _is either the_ <code>queries[i]<sup>th</sup></code> _smallest **positive palindrome** of length_ `intLength` _or_ `-1` _if no such palindrome exists_.

A **palindrome** is a number that reads the same backwards and forwards. Palindromes cannot have leading zeros.

**Example 1:**

**Input:** queries = [1,2,3,4,5,90], intLength = 3

**Output:** [101,111,121,131,141,999]

**Explanation:** 

The first few palindromes of length 3 are: 

101, 111, 121, 131, 141, 151, 161, 171, 181, 191, 202, ... 

The 90<sup>th</sup> palindrome of length 3 is 999.

**Example 2:**

**Input:** queries = [2,4,6], intLength = 4

**Output:** [1111,1331,1551]

**Explanation:** 

The first six palindromes of length 4 are: 

1001, 1111, 1221, 1331, 1441, and 1551.

**Constraints:**

*   <code>1 <= queries.length <= 5 * 10<sup>4</sup></code>
*   <code>1 <= queries[i] <= 10<sup>9</sup></code>
*   `1 <= intLength <= 15`

## Solution

```java
@SuppressWarnings("java:S2184")
public class Solution {
    public long[] kthPalindrome(int[] queries, int intLength) {
        long minHalf = (long) Math.pow(10, (intLength - 1) / 2);
        long maxIndex = (long) Math.pow(10, (intLength + 1) / 2) - minHalf;
        boolean isOdd = intLength % 2 == 1;
        long[] res = new long[queries.length];
        for (int i = 0; i < res.length; i++) {
            res[i] = queries[i] > maxIndex ? -1 : helper(queries[i], minHalf, isOdd);
        }
        return res;
    }

    private long helper(long index, long minHalf, boolean isOdd) {
        long half = minHalf + index - 1;
        long res = half;
        if (isOdd) {
            res /= 10;
        }
        while (half != 0) {
            res = res * 10 + half % 10;
            half /= 10;
        }
        return res;
    }
}
```