[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3072\. Distribute Elements Into Two Arrays II

Hard

You are given a **1-indexed** array of integers `nums` of length `n`.

We define a function `greaterCount` such that `greaterCount(arr, val)` returns the number of elements in `arr` that are **strictly greater** than `val`.

You need to distribute all the elements of `nums` between two arrays `arr1` and `arr2` using `n` operations. In the first operation, append `nums[1]` to `arr1`. In the second operation, append `nums[2]` to `arr2`. Afterwards, in the <code>i<sup>th</sup></code> operation:

*   If `greaterCount(arr1, nums[i]) > greaterCount(arr2, nums[i])`, append `nums[i]` to `arr1`.
*   If `greaterCount(arr1, nums[i]) < greaterCount(arr2, nums[i])`, append `nums[i]` to `arr2`.
*   If `greaterCount(arr1, nums[i]) == greaterCount(arr2, nums[i])`, append `nums[i]` to the array with a **lesser** number of elements.
*   If there is still a tie, append `nums[i]` to `arr1`.

The array `result` is formed by concatenating the arrays `arr1` and `arr2`. For example, if `arr1 == [1,2,3]` and `arr2 == [4,5,6]`, then `result = [1,2,3,4,5,6]`.

Return _the integer array_ `result`.

**Example 1:**

**Input:** nums = [2,1,3,3]

**Output:** [2,3,1,3]

**Explanation:** After the first 2 operations, arr1 = [2] and arr2 = [1]. 

In the 3<sup>rd</sup> operation, the number of elements greater than 3 is zero in both arrays.

Also, the lengths are equal, hence, append nums[3] to arr1. In the 4<sup>th</sup> operation, the number of elements greater than 3 is zero in both arrays. As the length of arr2 is lesser, hence, append nums[4] to arr2. 

After 4 operations, arr1 = [2,3] and arr2 = [1,3]. Hence, the array result formed by concatenation is [2,3,1,3].

**Example 2:**

**Input:** nums = [5,14,3,1,2]

**Output:** [5,3,1,2,14]

**Explanation:** After the first 2 operations, arr1 = [5] and arr2 = [14]. 

In the 3<sup>rd</sup> operation, the number of elements greater than 3 is one in both arrays. Also, the lengths are equal, hence, append nums[3] to arr1. 

In the 4<sup>th</sup> operation, the number of elements greater than 1 is greater in arr1 than arr2 (2 > 1). Hence, append nums[4] to arr1. In the 5<sup>th</sup> operation, the number of elements greater than 2 is greater in arr1 than arr2 (2 > 1). Hence, append nums[5] to arr1. 

zAfter 5 operations, arr1 = [5,3,1,2] and arr2 = [14]. Hence, the array result formed by concatenation is [5,3,1,2,14].

**Example 3:**

**Input:** nums = [3,3,3,3]

**Output:** [3,3,3,3]

**Explanation:** At the end of 4 operations, arr1 = [3,3] and arr2 = [3,3]. Hence, the array result formed by concatenation is [3,3,3,3].

**Constraints:**

*   <code>3 <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.Arrays;

public class Solution {
    private static class BIT {
        private final int[] tree;

        public BIT(int size) {
            tree = new int[size + 1];
        }

        public void update(int ind) {
            while (ind < tree.length) {
                tree[ind]++;
                ind += lsb(ind);
            }
        }

        public int rsq(int ind) {
            int sum = 0;
            while (ind > 0) {
                sum += tree[ind];
                ind -= lsb(ind);
            }
            return sum;
        }

        private int lsb(int n) {
            return n & (-n);
        }
    }

    public int[] resultArray(int[] source) {
        int[] nums = shrink(source);
        int[] arr1 = new int[nums.length];
        int[] arr2 = new int[nums.length];
        arr1[0] = source[0];
        arr2[0] = source[1];
        int p1 = 0;
        int p2 = 0;
        BIT bit1 = new BIT(nums.length);
        bit1.update(nums[0]);
        BIT bit2 = new BIT(nums.length);
        bit2.update(nums[1]);
        for (int i = 2; i < nums.length; i++) {
            int g1 = p1 + 1 - bit1.rsq(nums[i]);
            int g2 = p2 + 1 - bit2.rsq(nums[i]);
            if (g1 < g2 || p1 > p2) {
                p2++;
                arr2[p2] = source[i];
                bit2.update(nums[i]);
            } else {
                p1++;
                arr1[p1] = source[i];
                bit1.update(nums[i]);
            }
        }
        for (int i = p1 + 1; i < arr1.length; i++) {
            arr1[i] = arr2[i - p1 - 1];
        }
        return arr1;
    }

    private int[] shrink(int[] nums) {
        long[] b = new long[nums.length];
        for (int i = 0; i < nums.length; i++) {
            b[i] = (long) nums[i] << 32 | i;
        }
        Arrays.sort(b);
        int[] result = new int[nums.length];
        int p = 1;
        for (int i = 0; i < nums.length; i++) {
            if (i > 0 && (b[i] ^ b[i - 1]) >> 32 != 0) {
                p++;
            }
            result[(int) b[i]] = p;
        }
        return result;
    }
}
```