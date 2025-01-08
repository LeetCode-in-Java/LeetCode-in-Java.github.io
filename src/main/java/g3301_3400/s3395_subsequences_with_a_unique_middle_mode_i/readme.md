[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3395\. Subsequences with a Unique Middle Mode I

Hard

Given an integer array `nums`, find the number of subsequences of size 5 of `nums` with a **unique middle mode**.

Since the answer may be very large, return it **modulo** <code>10<sup>9</sup> + 7</code>.

A **mode** of a sequence of numbers is defined as the element that appears the **maximum** number of times in the sequence.

A sequence of numbers contains a **unique mode** if it has only one mode.

A sequence of numbers `seq` of size 5 contains a **unique middle mode** if the _middle element_ (`seq[2]`) is a **unique mode**.

A **subsequence** is a **non-empty** array that can be derived from another array by deleting some or no elements without changing the order of the remaining elements.

**Example 1:**

**Input:** nums = [1,1,1,1,1,1]

**Output:** 6

**Explanation:**

`[1, 1, 1, 1, 1]` is the only subsequence of size 5 that can be formed, and it has a unique middle mode of 1. This subsequence can be formed in 6 different ways, so the output is 6.

**Example 2:**

**Input:** nums = [1,2,2,3,3,4]

**Output:** 4

**Explanation:**

`[1, 2, 2, 3, 4]` and `[1, 2, 3, 3, 4]` each have a unique middle mode because the number at index 2 has the greatest frequency in the subsequence. `[1, 2, 2, 3, 3]` does not have a unique middle mode because 2 and 3 appear twice.

**Example 3:**

**Input:** nums = [0,1,2,3,4,5,6,7,8]

**Output:** 0

**Explanation:**

There is no subsequence of length 5 with a unique middle mode.

**Constraints:**

*   `5 <= nums.length <= 1000`
*   <code>-10<sup>9</sup> <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    private static final int MOD = (int) 1e9 + 7;
    private long[] c2 = new long[1001];

    public int subsequencesWithMiddleMode(int[] nums) {
        if (c2[2] == 0) {
            c2[0] = c2[1] = 0;
            c2[2] = 1;
            for (int i = 3; i < c2.length; ++i) {
                c2[i] = i * (i - 1) / 2;
            }
        }
        int n = nums.length;
        int[] newNums = new int[n];
        Map<Integer, Integer> map = new HashMap<>(n);
        int m = 0;
        int index = 0;
        for (int x : nums) {
            Integer id = map.get(x);
            if (id == null) {
                id = m++;
                map.put(x, id);
            }
            newNums[index++] = id;
        }
        if (m == n) {
            return 0;
        }
        int[] rightCount = new int[m];
        for (int x : newNums) {
            rightCount[x]++;
        }
        int[] leftCount = new int[m];
        long ans = (long) n * (n - 1) * (n - 2) * (n - 3) * (n - 4) / 120;
        for (int left = 0; left < n - 2; left++) {
            int x = newNums[left];
            rightCount[x]--;
            if (left >= 2) {
                int right = n - (left + 1);
                int leftX = leftCount[x];
                int rightX = rightCount[x];
                ans -= c2[left - leftX] * c2[right - rightX];
                for (int y = 0; y < m; ++y) {
                    if (y == x) {
                        continue;
                    }
                    int rightY = rightCount[y];
                    int leftY = leftCount[y];
                    ans -= c2[leftY] * rightX * (right - rightX);
                    ans -= c2[rightY] * leftX * (left - leftX);
                    ans -=
                            leftY
                                    * rightY
                                    * (leftX * (right - rightX - rightY)
                                            + rightX * (left - leftX - leftY));
                }
            }
            leftCount[x]++;
        }
        return (int) (ans % MOD);
    }
}
```