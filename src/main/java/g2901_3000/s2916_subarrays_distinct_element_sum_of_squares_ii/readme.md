[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2916\. Subarrays Distinct Element Sum of Squares II

Hard

You are given a **0-indexed** integer array `nums`.

The **distinct count** of a subarray of `nums` is defined as:

*   Let `nums[i..j]` be a subarray of `nums` consisting of all the indices from `i` to `j` such that `0 <= i <= j < nums.length`. Then the number of distinct values in `nums[i..j]` is called the distinct count of `nums[i..j]`.

Return _the sum of the **squares** of **distinct counts** of all subarrays of_ `nums`.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A subarray is a contiguous **non-empty** sequence of elements within an array.

**Example 1:**

**Input:** nums = [1,2,1]

**Output:** 15

**Explanation:** Six possible subarrays are: 

[1]: 1 distinct value 

[2]: 1 distinct value 

[1]: 1 distinct value 

[1,2]: 2 distinct values

[2,1]: 2 distinct values

[1,2,1]: 2 distinct values

The sum of the squares of the distinct counts in all subarrays is equal to 1<sup>2</sup> + 1<sup>2</sup> + 1<sup>2</sup> + 2<sup>2</sup> + 2<sup>2</sup> + 2<sup>2</sup> = 15.

**Example 2:**

**Input:** nums = [2,2]

**Output:** 3

**Explanation:** Three possible subarrays are: 

[2]: 1 distinct value 

[2]: 1 distinct value

[2,2]: 1 distinct value 

The sum of the squares of the distinct counts in all subarrays is equal to 1<sup>2</sup> + 1<sup>2</sup> + 1<sup>2</sup> = 3.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    private static final int MOD = (int) (1e9) + 7;
    private int n;
    private long[] tree1;
    private long[] tree2;

    public int sumCounts(int[] nums) {
        n = nums.length;
        tree1 = new long[n + 1];
        tree2 = new long[n + 1];
        int max = 0;
        for (int x : nums) {
            if (x > max) {
                max = x;
            }
        }
        int[] last = new int[max + 1];
        long ans = 0;
        long cur = 0;
        for (int i = 1; i <= n; i++) {
            int x = nums[i - 1];
            int j = last[x];
            cur += 2 * (query(i) - query(j)) + (i - j);
            ans += cur;
            update(j + 1, 1);
            update(i + 1, -1);
            last[x] = i;
        }
        return (int) (ans % MOD);
    }

    int lowbit(int index) {
        return index & (-index);
    }

    void update(int index, int x) {
        int v = index * x;
        while (index <= n) {
            tree1[index] += x;
            tree2[index] += v;
            index += lowbit(index);
        }
    }

    long query(int index) {
        long res = 0;
        int p = index + 1;
        while (index > 0) {
            res += p * tree1[index] - tree2[index];
            index -= lowbit(index);
        }
        return res;
    }
}
```