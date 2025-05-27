[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3556\. Sum of Largest Prime Substrings

Medium

Given a string `s`, find the sum of the **3 largest unique prime numbers** that can be formed using any of its ****substring****.

Return the **sum** of the three largest unique prime numbers that can be formed. If fewer than three exist, return the sum of **all** available primes. If no prime numbers can be formed, return 0.

**Note:** Each prime number should be counted only **once**, even if it appears in **multiple** substrings. Additionally, when converting a substring to an integer, any leading zeros are ignored.

**Example 1:**

**Input:** s = "12234"

**Output:** 1469

**Explanation:**

*   The unique prime numbers formed from the substrings of `"12234"` are 2, 3, 23, 223, and 1223.
*   The 3 largest primes are 1223, 223, and 23. Their sum is 1469.

**Example 2:**

**Input:** s = "111"

**Output:** 11

**Explanation:**

*   The unique prime number formed from the substrings of `"111"` is 11.
*   Since there is only one prime number, the sum is 11.

**Constraints:**

*   `1 <= s.length <= 10`
*   `s` consists of only digits.

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public long sumOfLargestPrimes(String s) {
        Set<Long> set = new HashSet<>();
        int n = s.length();
        long first = -1;
        long second = -1;
        long third = -1;
        for (int i = 0; i < n; i++) {
            long num = 0;
            for (int j = i; j < n; j++) {
                num = num * 10 + (s.charAt(j) - '0');
                if (i != j && s.charAt(i) == '0') {
                    break;
                }
                if (isPrime(num) && !set.contains(num)) {
                    set.add(num);
                    if (num > first) {
                        third = second;
                        second = first;
                        first = num;
                    } else if (num > second) {
                        third = second;
                        second = num;
                    } else if (num > third) {
                        third = num;
                    }
                }
            }
        }
        long sum = 0;
        if (first != -1) {
            sum += first;
        }
        if (second != -1) {
            sum += second;
        }
        if (third != -1) {
            sum += third;
        }
        return sum;
    }

    public boolean isPrime(long num) {
        if (num <= 1) {
            return false;
        }
        if (num == 2 || num == 3) {
            return true;
        }
        if (num % 2 == 0 || num % 3 == 0) {
            return false;
        }
        for (long i = 5; i * i <= num; i += 6) {
            if (num % i == 0 || num % (i + 2) == 0) {
                return false;
            }
        }
        return true;
    }
}
```