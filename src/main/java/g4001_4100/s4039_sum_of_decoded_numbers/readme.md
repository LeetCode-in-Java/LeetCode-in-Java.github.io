[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4039\. Sum of Decoded Numbers

Medium

You are given an integer array `nums`.

Each `nums[i]` is an **encoded** integer representing two positive integers <code>x<sub>i</sub></code> and <code>y<sub>i</sub></code>. To decode `nums[i]`, define:

*   <code>width<sub>i</sub> = nums[i] % 10</code>.
*   <code>d<sub>i</sub> = floor(nums[i] / 10)</code>.
*   <code>x<sub>i</sub></code> as the integer formed by the first <code>width<sub>i</sub></code> digits of the decimal representation of <code>d<sub>i</sub></code>.
*   <code>y<sub>i</sub></code> as the integer formed by all remaining digits of the decimal representation of <code>d<sub>i</sub></code>.

It is guaranteed that the decimal representation of <code>d<sub>i</sub></code> contains more than <code>width<sub>i</sub></code> digits. Therefore, both <code>x<sub>i</sub></code> and <code>y<sub>i</sub></code> contain at least one digit.

The **decoded value** of `nums[i]` is <code>x<sub>i</sub><sup>y<sub>i</sub></sup></code>.

Return the sum of the decoded values of all elements in `nums`, modulo <code>10<sup>9</sup> + 7</code>.

The `floor()` function returns the integer part of the division.

**Example 1:**

**Input:** nums = [231]

**Output:** 8

**Explanation:**

*   For 231, we have `width = 1`, `d = 23`, `x = 2`, and `y = 3`.
*   The decoded value of 231 is <code>2<sup>3</sup> = 8</code>.
*   Since there is only one element in `nums`, the sum of the decoded values is 8.

**Example 2:**

**Input:** nums = [2522,2101]

**Output:** 1649

**Explanation:**

*   For 2522, we have `width = 2`, `d = 252`, `x = 25`, and `y = 2`.
*   The decoded value of 2522 is <code>25<sup>2</sup> = 625</code>.
*   For 2101, we have `width = 1`, `d = 210`, `x = 2`, and `y = 10`.
*   The decoded value of 2101 is <code>2<sup>10</sup> = 1024</code>.
*   The sum of the decoded values is `625 + 1024 = 1649`.

**Example 3:**

**Input:** nums = [2301]

**Output:** 73741817

**Explanation:**

*   For 2301, we have `width = 1`, `d = 230`, `x = 2`, and `y = 30`.
*   The decoded value is <code>2<sup>30</sup> = 1073741824</code>.
*   Therefore, the answer is <code>1073741824 modulo (10<sup>9</sup> + 7) = 73741817</code>.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>100 < nums[i] < 10<sup>15</sup></code>
*   <code>1 <= width<sub>i</sub> <= 9</code>
*   <code>1 <= x<sub>i</sub>, y<sub>i</sub> < 10<sup>9</sup></code>
*   The digit sequences used to form <code>x<sub>i</sub></code> and <code>y<sub>i</sub></code> do not have leading zeros.
*   It is guaranteed that every element in `nums` is a valid encoded integer.

## Solution

```java
public class Solution {
    private static final int MOD = (int) 1e9 + 7;
    private static final int N = 16;
    private final long[] pow10 = new long[N];

    public int sumDecoded(long[] nums) {
        if (pow10[0] == 0) {
            pow10[0] = 1;
            for (int i = 1; i < N; ++i) {
                pow10[i] = pow10[i - 1] * 10;
            }
        }
        long sum = 0;
        for (long num : nums) {
            int w = (int) (num % 10);
            long v = num / 10;
            int len = 1;
            while (v >= pow10[len]) {
                len++;
            }
            long base = pow10[len - w];
            long x = v / base;
            long y = v % base;
            sum += pow(x, y);
        }
        return (int) (sum % MOD);
    }

    private long pow(long x, long n) {
        long result = 1;
        while (n > 0) {
            if ((n & 1) == 1) {
                result = result * x % MOD;
            }
            x = x * x % MOD;
            n >>= 1;
        }
        return result;
    }
}
```