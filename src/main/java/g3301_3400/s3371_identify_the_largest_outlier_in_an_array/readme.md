[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3371\. Identify the Largest Outlier in an Array

Medium

You are given an integer array `nums`. This array contains `n` elements, where **exactly** `n - 2` elements are **special** **numbers**. One of the remaining **two** elements is the _sum_ of these **special numbers**, and the other is an **outlier**.

An **outlier** is defined as a number that is _neither_ one of the original special numbers _nor_ the element representing the sum of those numbers.

**Note** that special numbers, the sum element, and the outlier must have **distinct** indices, but _may_ share the **same** value.

Return the **largest** potential **outlier** in `nums`.

**Example 1:**

**Input:** nums = [2,3,5,10]

**Output:** 10

**Explanation:**

The special numbers could be 2 and 3, thus making their sum 5 and the outlier 10.

**Example 2:**

**Input:** nums = [-2,-1,-3,-6,4]

**Output:** 4

**Explanation:**

The special numbers could be -2, -1, and -3, thus making their sum -6 and the outlier 4.

**Example 3:**

**Input:** nums = [1,1,1,1,1,5,5]

**Output:** 5

**Explanation:**

The special numbers could be 1, 1, 1, 1, and 1, thus making their sum 5 and the other 5 as the outlier.

**Constraints:**

*   <code>3 <= nums.length <= 10<sup>5</sup></code>
*   `-1000 <= nums[i] <= 1000`
*   The input is generated such that at least **one** potential outlier exists in `nums`.

## Solution

```java
public class Solution {
    public int getLargestOutlier(int[] nums) {
        int[] cnt = new int[2001];
        int sum = 0;
        for (int i : nums) {
            sum += i;
            cnt[i + 1000]++;
        }
        for (int i = cnt.length - 1; i >= 0; --i) {
            int j = i - 1000;
            if (cnt[i] == 0) {
                continue;
            }
            sum -= j;
            int csum = (sum >> 1) + 1000;
            cnt[i]--;
            if (sum % 2 == 0 && csum >= 0 && csum < cnt.length && cnt[csum] > 0) {
                return j;
            }
            sum += j;
            cnt[i]++;
        }
        return 0;
    }
}
```