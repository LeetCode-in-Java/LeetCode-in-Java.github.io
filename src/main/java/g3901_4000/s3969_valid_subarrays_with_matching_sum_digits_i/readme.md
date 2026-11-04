[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3969\. Valid Subarrays With Matching Sum Digits I

Medium

You are given an integer array `nums` and an integer digit `x`.

A non-empty subarrays `nums[l..r]` is considered **valid** if the sum of its elements satisfies both of the following conditions:

*   The first digit of the sum is equal to `x`.
*   The last digit of the sum is equal to `x`.

Return the number of valid subarrays.

**Example 1:**

**Input:** nums = [1,100,1], x = 1

**Output:** 4

**Explanation:**

The valid subarrays are:

*   `nums[0..0]`: `sum = 1`
*   `nums[0..1]`: `sum = 1 + 100 = 101`
*   `nums[1..2]`: `sum = 100 + 1 = 101`
*   `nums[2..2]`: `sum = 1`

Thus, the answer is 4.

**Example 2:**

**Input:** nums = [1], x = 2

**Output:** 0

**Explanation:**

The only subarray is `nums[0..0]` with a sum of 1, which does not satisfy the conditions.

Thus, the answer is 0.

**Constraints:**

*   `1 <= nums.length <= 1500`
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= x <= 9`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

@SuppressWarnings("unchecked")
public class Solution {
    public int countValidSubarrays(int[] nums, int x) {
        int n = nums.length;
        long[] pre = new long[n + 1];
        for (int i = 0; i < n; i++) {
            pre[i + 1] = pre[i] + nums[i];
        }
        if (x == 0) {
            return 0;
        }
        List<long[]> interval = new ArrayList<>();
        for (long p = 1; p <= pre[n]; p *= 10) {
            interval.add(new long[] {p * x, p * x + p - 1});
        }
        List[] isit = new List[10];
        for (int r = 0; r < 10; r++) {
            isit[r] = new ArrayList<>();
        }
        long sum = 0;
        for (int i = 0; i <= n; i++) {
            if (i > 0) {
                long k = pre[i];
                int r = (int) (((k - x) % 10 + 10) % 10);
                List<Long> list = isit[r];
                for (long[] j : interval) {
                    long a = k - j[1];
                    long b = k - j[0];
                    if (b < 0) {
                        continue;
                    }
                    int high = upper(list, b);
                    int low = lower(list, a);
                    sum += high - low;
                }
            }
            isit[(int) (pre[i] % 10)].add(pre[i]);
        }
        return (int) sum;
    }

    private int lower(List<Long> list, long k) {
        int l = 0;
        int h = list.size();
        while (l < h) {
            int m = (l + h) >>> 1;
            if (list.get(m) < k) {
                l = m + 1;
            } else {
                h = m;
            }
        }
        return l;
    }

    private int upper(List<Long> list, long k) {
        int l = 0;
        int h = list.size();
        while (l < h) {
            int m = (l + h) >>> 1;
            if (list.get(m) <= k) {
                l = m + 1;
            } else {
                h = m;
            }
        }
        return l;
    }
}
```