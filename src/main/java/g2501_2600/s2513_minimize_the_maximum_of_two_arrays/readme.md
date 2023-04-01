[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2513\. Minimize the Maximum of Two Arrays

Medium

We have two arrays `arr1` and `arr2` which are initially empty. You need to add positive integers to them such that they satisfy all the following conditions:

*   `arr1` contains `uniqueCnt1` **distinct** positive integers, each of which is **not divisible** by `divisor1`.
*   `arr2` contains `uniqueCnt2` **distinct** positive integers, each of which is **not divisible** by `divisor2`.
*   **No** integer is present in both `arr1` and `arr2`.

Given `divisor1`, `divisor2`, `uniqueCnt1`, and `uniqueCnt2`, return _the **minimum possible maximum** integer that can be present in either array_.

**Example 1:**

**Input:** divisor1 = 2, divisor2 = 7, uniqueCnt1 = 1, uniqueCnt2 = 3

**Output:** 4

**Explanation:**

We can distribute the first 4 natural numbers into arr1 and arr2. 

arr1 = [1] and arr2 = [2,3,4]. 

We can see that both arrays satisfy all the conditions. 

Since the maximum value is 4, we return it.

**Example 2:**

**Input:** divisor1 = 3, divisor2 = 5, uniqueCnt1 = 2, uniqueCnt2 = 1

**Output:** 3

**Explanation:** 

Here arr1 = [1,2], and arr2 = [3] satisfy all conditions. 

Since the maximum value is 3, we return it.

**Example 3:**

**Input:** divisor1 = 2, divisor2 = 4, uniqueCnt1 = 8, uniqueCnt2 = 2

**Output:** 15

**Explanation:** 

Here, the final possible arrays can be arr1 = [1,3,5,7,9,11,13,15], and arr2 = [2,6]. 

It can be shown that it is not possible to obtain a lower maximum satisfying all conditions.

**Constraints:**

*   <code>2 <= divisor1, divisor2 <= 10<sup>5</sup></code>
*   <code>1 <= uniqueCnt1, uniqueCnt2 < 10<sup>9</sup></code>
*   <code>2 <= uniqueCnt1 + uniqueCnt2 <= 10<sup>9</sup></code>

## Solution

```java
public class Solution {
    private int gcd(int a, int b) {
        if (b == 0) {
            return a;
        }
        return gcd(b, a % b);
    }

    public int minimizeSet(int divisor1, int divisor2, int uniqueCnt1, int uniqueCnt2) {
        long lo = 1;
        long hi = (int) 10e10;
        if (divisor2 == 0) {
            divisor2 = 1;
        }
        long lcm = ((long) divisor1 * (long) divisor2) / gcd(divisor1, divisor2);
        while (lo < hi) {
            long mi = lo + (hi - lo) / 2;
            int cnt1 = (int) (mi - mi / divisor1);
            int cnt2 = (int) (mi - mi / divisor2);
            int cntAll = (int) (mi - mi / lcm);
            if (cnt1 < uniqueCnt1 || cnt2 < uniqueCnt2 || cntAll < uniqueCnt1 + uniqueCnt2) {
                lo = mi + 1;
            } else {
                hi = mi;
            }
        }
        return (int) lo;
    }
}
```