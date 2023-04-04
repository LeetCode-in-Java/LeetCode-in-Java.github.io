[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 1399\. Count Largest Group

Easy

You are given an integer `n`.

Each number from `1` to `n` is grouped according to the sum of its digits.

Return _the number of groups that have the largest size_.

**Example 1:**

**Input:** n = 13

**Output:** 4

**Explanation:** There are 9 groups in total, they are grouped according sum of its digits of numbers from 1 to 13:

[1,10], [2,11], [3,12], [4,13], [5], [6], [7], [8], [9].

There are 4 groups with largest size.

**Example 2:**

**Input:** n = 2

**Output:** 2

**Explanation:** There are 2 groups [1], [2] of size 1.

**Constraints:**

*   <code>1 <= n <= 10<sup>4</sup></code>

## Solution

```java
public class Solution {
    public int countLargestGroup(int n) {
        int largest = 0;
        int[] map = new int[37];
        int sumOfDigit = 0;
        for (int i = 1; i <= n; i++) {
            if (i % 10 == 0) {
                // reset and start a new sum
                sumOfDigit = getSumOfDigits(i);
            } else {
                sumOfDigit++;
            }
            int val = ++map[sumOfDigit];
            largest = val > largest ? val : largest;
        }
        return countLargestGroup(largest, map);
    }

    private int countLargestGroup(int largest, int[] arr) {
        int count = 0;
        for (int val : arr) {
            if (val == largest) {
                count++;
            }
        }
        return count;
    }

    private int getSumOfDigits(int num) {
        int sum = 0;
        while (num > 0) {
            sum += num % 10;
            num /= 10;
        }
        return sum;
    }
}
```