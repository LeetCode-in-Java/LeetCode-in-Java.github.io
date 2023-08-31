[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2605\. Form Smallest Number From Two Digit Arrays

Easy

Given two arrays of **unique** digits `nums1` and `nums2`, return _the **smallest** number that contains **at least** one digit from each array_.

**Example 1:**

**Input:** nums1 = [4,1,3], nums2 = [5,7]

**Output:** 15

**Explanation:** The number 15 contains the digit 1 from nums1 and the digit 5 from nums2. It can be proven that 15 is the smallest number we can have.

**Example 2:**

**Input:** nums1 = [3,5,2,6], nums2 = [3,1,7]

**Output:** 3

**Explanation:** The number 3 contains the digit 3 which exists in both arrays.

**Constraints:**

*   `1 <= nums1.length, nums2.length <= 9`
*   `1 <= nums1[i], nums2[i] <= 9`
*   All digits in each array are **unique**.

## Solution

```java
import java.util.Arrays;

public class Solution {
    public int minNumber(int[] nums1, int[] nums2) {
        Arrays.sort(nums1);
        Arrays.sort(nums2);
        int[] temp = new int[Math.max(nums1[nums1.length - 1], nums2[nums2.length - 1]) + 1];
        int n1 = nums1[0];
        int n2 = nums2[0];
        int k = Math.min(n1 * 10 + n2, n2 * 10 + n1);
        for (int value : nums1) {
            temp[value]++;
        }
        for (int i : nums2) {
            if (temp[i] > 0) {
                k = Math.min(k, i);
                return k;
            }
        }
        return k;
    }
}
```