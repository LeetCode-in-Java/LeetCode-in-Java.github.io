[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3574\. Maximize Subarray GCD Score

Hard

You are given an array of positive integers `nums` and an integer `k`.

You may perform at most `k` operations. In each operation, you can choose one element in the array and **double** its value. Each element can be doubled **at most** once.

The **score** of a contiguous **subarray** is defined as the **product** of its length and the _greatest common divisor (GCD)_ of all its elements.

Your task is to return the **maximum** **score** that can be achieved by selecting a contiguous subarray from the modified array.

**Note:**

*   The **greatest common divisor (GCD)** of an array is the largest integer that evenly divides all the array elements.

**Example 1:**

**Input:** nums = [2,4], k = 1

**Output:** 8

**Explanation:**

*   Double `nums[0]` to 4 using one operation. The modified array becomes `[4, 4]`.
*   The GCD of the subarray `[4, 4]` is 4, and the length is 2.
*   Thus, the maximum possible score is `2 × 4 = 8`.

**Example 2:**

**Input:** nums = [3,5,7], k = 2

**Output:** 14

**Explanation:**

*   Double `nums[2]` to 14 using one operation. The modified array becomes `[3, 5, 14]`.
*   The GCD of the subarray `[14]` is 14, and the length is 1.
*   Thus, the maximum possible score is `1 × 14 = 14`.

**Example 3:**

**Input:** nums = [5,5,5], k = 1

**Output:** 15

**Explanation:**

*   The subarray `[5, 5, 5]` has a GCD of 5, and its length is 3.
*   Since doubling any element doesn't improve the score, the maximum score is `3 × 5 = 15`.

**Constraints:**

*   `1 <= n == nums.length <= 1500`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= k <= n`

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

@SuppressWarnings("unchecked")
public class Solution {
    public long maxGCDScore(int[] nums, int k) {
        int mx = 0;
        for (int x : nums) {
            mx = Math.max(mx, x);
        }
        int width = 32 - Integer.numberOfLeadingZeros(mx);
        List<Integer>[] lowbitPos = new List[width];
        Arrays.setAll(lowbitPos, i -> new ArrayList<>());
        int[][] intervals = new int[width + 1][3];
        int size = 0;
        long ans = 0;
        for (int i = 0; i < nums.length; i++) {
            int x = nums[i];
            int tz = Integer.numberOfTrailingZeros(x);
            lowbitPos[tz].add(i);
            for (int j = 0; j < size; j++) {
                intervals[j][0] = gcd(intervals[j][0], x);
            }
            intervals[size][0] = x;
            intervals[size][1] = i - 1;
            intervals[size][2] = i;
            size++;
            int idx = 1;
            for (int j = 1; j < size; j++) {
                if (intervals[j][0] != intervals[j - 1][0]) {
                    intervals[idx][0] = intervals[j][0];
                    intervals[idx][1] = intervals[j][1];
                    intervals[idx][2] = intervals[j][2];
                    idx++;
                } else {
                    intervals[idx - 1][2] = intervals[j][2];
                }
            }
            size = idx;
            for (int j = 0; j < size; j++) {
                int g = intervals[j][0];
                int l = intervals[j][1];
                int r = intervals[j][2];
                ans = Math.max(ans, (long) g * (i - l));
                List<Integer> pos = lowbitPos[Integer.numberOfTrailingZeros(g)];
                int minL = pos.size() > k ? Math.max(l, pos.get(pos.size() - k - 1)) : l;
                if (minL < r) {
                    ans = Math.max(ans, (long) g * 2 * (i - minL));
                }
            }
        }
        return ans;
    }

    private int gcd(int a, int b) {
        while (a != 0) {
            int tmp = a;
            a = b % a;
            b = tmp;
        }
        return b;
    }
}
```