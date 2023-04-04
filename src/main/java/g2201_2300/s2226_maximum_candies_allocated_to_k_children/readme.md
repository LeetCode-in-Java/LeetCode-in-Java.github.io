[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2226\. Maximum Candies Allocated to K Children

Medium

You are given a **0-indexed** integer array `candies`. Each element in the array denotes a pile of candies of size `candies[i]`. You can divide each pile into any number of **sub piles**, but you **cannot** merge two piles together.

You are also given an integer `k`. You should allocate piles of candies to `k` children such that each child gets the **same** number of candies. Each child can take **at most one** pile of candies and some piles of candies may go unused.

Return _the **maximum number of candies** each child can get._

**Example 1:**

**Input:** candies = [5,8,6], k = 3

**Output:** 5

**Explanation:** We can divide candies[1] into 2 piles of size 5 and 3, and candies[2] into 2 piles of size 5 and 1. We now have five piles of candies of sizes 5, 5, 3, 5, and 1. We can allocate the 3 piles of size 5 to 3 children. It can be proven that each child cannot receive more than 5 candies. 

**Example 2:**

**Input:** candies = [2,5], k = 11

**Output:** 0

**Explanation:** There are 11 children but only 7 candies in total, so it is impossible to ensure each child receives at least one candy. Thus, each child gets no candy and the answer is 0. 

**Constraints:**

*   <code>1 <= candies.length <= 10<sup>5</sup></code>
*   <code>1 <= candies[i] <= 10<sup>7</sup></code>
*   <code>1 <= k <= 10<sup>12</sup></code>

## Solution

```java
public class Solution {
    public int maximumCandies(int[] candies, long k) {
        int max = Integer.MIN_VALUE;
        long sum = 0;
        for (int num : candies) {
            max = Math.max(max, num);
            sum += num;
        }
        if (sum < k) {
            return 0;
        }
        int left = 1;
        int right = max;
        while (left < right) {
            int mid = left + (right - left) / 2;
            if (canBeDistributed(mid, candies, k)) {
                if (!canBeDistributed(mid + 1, candies, k)) {
                    return mid;
                } else {
                    left = mid + 1;
                }
            } else {
                right = mid - 1;
            }
        }
        return left;
    }

    private boolean canBeDistributed(int num, int[] candies, long k) {
        for (int i = 0; i < candies.length && k > 0; ++i) {
            k -= candies[i] / num;
        }
        return k <= 0;
    }
}
```