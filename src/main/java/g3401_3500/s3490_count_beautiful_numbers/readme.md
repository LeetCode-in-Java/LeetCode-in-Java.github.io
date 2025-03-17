[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3490\. Count Beautiful Numbers

Hard

You are given two positive integers, `l` and `r`. A positive integer is called **beautiful** if the product of its digits is divisible by the sum of its digits.

Return the count of **beautiful** numbers between `l` and `r`, inclusive.

**Example 1:**

**Input:** l = 10, r = 20

**Output:** 2

**Explanation:**

The beautiful numbers in the range are 10 and 20.

**Example 2:**

**Input:** l = 1, r = 15

**Output:** 10

**Explanation:**

The beautiful numbers in the range are 1, 2, 3, 4, 5, 6, 7, 8, 9, and 10.

**Constraints:**

*   <code>1 <= l <= r < 10<sup>9</sup></code>

## Solution

```java
import java.util.HashMap;

public class Solution {
    public int beautifulNumbers(int l, int r) {
        return countBeautiful(r) - countBeautiful(l - 1);
    }

    private int countBeautiful(int x) {
        char[] digits = getCharArray(x);
        HashMap<String, Integer> dp = new HashMap<>();
        return solve(0, 1, 0, 1, digits, dp);
    }

    private char[] getCharArray(int x) {
        String str = String.valueOf(x);
        return str.toCharArray();
    }

    private int solve(
            int i, int tight, int sum, int prod, char[] digits, HashMap<String, Integer> dp) {
        if (i == digits.length) {
            if (sum > 0 && prod % sum == 0) {
                return 1;
            } else {
                return 0;
            }
        }
        String str = i + " - " + tight + " - " + sum + " - " + prod;
        if (dp.containsKey(str)) {
            return dp.get(str);
        }
        int limit;
        if (tight == 1) {
            limit = digits[i] - '0';
        } else {
            limit = 9;
        }
        int count = 0;
        int j = 0;
        while (j <= limit) {
            int newTight = 0;
            if (tight == 1 && j == limit) {
                newTight = 1;
            }
            int newSum = sum + j;
            int newProd;
            if (j == 0 && sum == 0) {
                newProd = 1;
            } else {
                newProd = prod * j;
            }
            count += solve(i + 1, newTight, newSum, newProd, digits, dp);
            j++;
        }
        dp.put(str, count);
        return count;
    }
}
```