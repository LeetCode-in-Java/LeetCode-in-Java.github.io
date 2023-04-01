[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 1818\. Minimum Absolute Sum Difference

Medium

You are given two positive integer arrays `nums1` and `nums2`, both of length `n`.

The **absolute sum difference** of arrays `nums1` and `nums2` is defined as the **sum** of `|nums1[i] - nums2[i]|` for each `0 <= i < n` (**0-indexed**).

You can replace **at most one** element of `nums1` with **any** other element in `nums1` to **minimize** the absolute sum difference.

Return the _minimum absolute sum difference **after** replacing at most one element in the array `nums1`._ Since the answer may be large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

`|x|` is defined as:

*   `x` if `x >= 0`, or
*   `-x` if `x < 0`.

**Example 1:**

**Input:** nums1 = [1,7,5], nums2 = [2,3,5]

**Output:** 3

**Explanation:** There are two possible optimal solutions: 

- Replace the second element with the first: [1,**7**,5] => [1,**1**,5], or 

- Replace the second element with the third: [1,**7**,5] => [1,**5**,5]. 
  
Both will yield an absolute sum difference of `|1-2| + (|1-3| or |5-3|) + |5-5| =` 3\.

**Example 2:**

**Input:** nums1 = [2,4,6,8,10], nums2 = [2,4,6,8,10]

**Output:** 0

**Explanation:** nums1 is equal to nums2 so no replacement is needed. This will result in an absolute sum difference of 0.

**Example 3:**

**Input:** nums1 = [1,10,4,4,2,7], nums2 = [9,3,5,1,7,4]

**Output:** 20

**Explanation:** Replace the first element with the second: [**1**,10,4,4,2,7] => [**10**,10,4,4,2,7]. This yields an absolute sum difference of `|10-9| + |10-3| + |4-5| + |4-1| + |2-7| + |7-4| = 20`

**Constraints:**

*   `n == nums1.length`
*   `n == nums2.length`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums1[i], nums2[i] <= 10<sup>5</sup></code>

## Solution

```java
public class Solution {
    public int minAbsoluteSumDiff(int[] nums1, int[] nums2) {
        int min = Integer.MAX_VALUE;
        int max = Integer.MIN_VALUE;
        for (int i = 0; i < nums1.length; i++) {
            min = Math.min(min, Math.min(nums1[i], nums2[i]));
            max = Math.max(max, Math.max(nums1[i], nums2[i]));
        }
        int[] less = new int[max - min + 1];
        int[] more = new int[max - min + 1];
        less[0] = -max - 1;
        more[more.length - 1] = ((max + 1) << 1);
        for (int num : nums1) {
            less[num - min] = num;
            more[num - min] = num;
        }
        for (int i = 1; i < less.length; i++) {
            if (less[i] == 0) {
                less[i] = less[i - 1];
            }
        }
        for (int i = more.length - 2; i >= 0; i--) {
            if (more[i] == 0) {
                more[i] = more[i + 1];
            }
        }
        int total = 0;
        int preSave = 0;
        for (int i = 0; i < nums1.length; i++) {
            int current = Math.abs(nums1[i] - nums2[i]);
            total += current;
            int save =
                    current
                            - Math.min(
                                    Math.abs(less[nums2[i] - min] - nums2[i]),
                                    Math.abs(more[nums2[i] - min] - nums2[i]));
            if (save > preSave) {
                total = total + preSave - save;
                preSave = save;
            }
            total = total % 1_000_000_007;
        }
        return total;
    }
}
```