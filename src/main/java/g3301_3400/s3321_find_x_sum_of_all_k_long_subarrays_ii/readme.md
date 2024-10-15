[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3321\. Find X-Sum of All K-Long Subarrays II

Hard

You are given an array `nums` of `n` integers and two integers `k` and `x`.

The **x-sum** of an array is calculated by the following procedure:

*   Count the occurrences of all elements in the array.
*   Keep only the occurrences of the top `x` most frequent elements. If two elements have the same number of occurrences, the element with the **bigger** value is considered more frequent.
*   Calculate the sum of the resulting array.

**Note** that if an array has less than `x` distinct elements, its **x-sum** is the sum of the array.

Return an integer array `answer` of length `n - k + 1` where `answer[i]` is the **x-sum** of the subarray `nums[i..i + k - 1]`.

**Example 1:**

**Input:** nums = [1,1,2,2,3,4,2,3], k = 6, x = 2

**Output:** [6,10,12]

**Explanation:**

*   For subarray `[1, 1, 2, 2, 3, 4]`, only elements 1 and 2 will be kept in the resulting array. Hence, `answer[0] = 1 + 1 + 2 + 2`.
*   For subarray `[1, 2, 2, 3, 4, 2]`, only elements 2 and 4 will be kept in the resulting array. Hence, `answer[1] = 2 + 2 + 2 + 4`. Note that 4 is kept in the array since it is bigger than 3 and 1 which occur the same number of times.
*   For subarray `[2, 2, 3, 4, 2, 3]`, only elements 2 and 3 are kept in the resulting array. Hence, `answer[2] = 2 + 2 + 2 + 3 + 3`.

**Example 2:**

**Input:** nums = [3,8,7,8,7,5], k = 2, x = 2

**Output:** [11,15,15,15,12]

**Explanation:**

Since `k == x`, `answer[i]` is equal to the sum of the subarray `nums[i..i + k - 1]`.

**Constraints:**

*   `nums.length == n`
*   <code>1 <= n <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>
*   `1 <= x <= k <= nums.length`

## Solution

```java
import java.util.HashMap;
import java.util.Map;
import java.util.TreeSet;

@SuppressWarnings("java:S1210")
public class Solution {
    private static class RC implements Comparable<RC> {
        int val;
        int cnt;

        RC(int v, int c) {
            val = v;
            cnt = c;
        }

        public int compareTo(RC o) {
            if (cnt != o.cnt) {
                return cnt - o.cnt;
            }
            return val - o.val;
        }
    }

    public long[] findXSum(int[] nums, int k, int x) {
        int n = nums.length;
        long[] ans = new long[n - k + 1];
        Map<Integer, Integer> cnt = new HashMap<>();
        TreeSet<RC> s1 = new TreeSet<>();
        TreeSet<RC> s2 = new TreeSet<>();
        long sum = 0;
        long xSum = 0;
        for (int i = 0; i < n; ++i) {
            sum += nums[i];
            int curCnt = cnt.getOrDefault(nums[i], 0);
            cnt.put(nums[i], curCnt + 1);
            RC tmp = new RC(nums[i], curCnt);
            if (s1.contains(tmp)) {
                s1.remove(tmp);
                s1.add(new RC(nums[i], curCnt + 1));
                xSum += nums[i];
            } else {
                s2.remove(tmp);
                s1.add(new RC(nums[i], curCnt + 1));
                xSum += (long) nums[i] * (curCnt + 1);
                while (s1.size() > x) {
                    RC l = s1.first();
                    s1.remove(l);
                    xSum -= (long) l.val * l.cnt;
                    s2.add(l);
                }
            }
            if (i >= k - 1) {
                ans[i - k + 1] = s1.size() == x ? xSum : sum;
                int v = nums[i - k + 1];
                sum -= v;
                curCnt = cnt.get(v);
                if (curCnt > 1) {
                    cnt.put(v, curCnt - 1);
                } else {
                    cnt.remove(v);
                }
                tmp = new RC(v, curCnt);
                if (s2.contains(tmp)) {
                    s2.remove(tmp);
                    if (curCnt > 1) {
                        s2.add(new RC(v, curCnt - 1));
                    }
                } else {
                    s1.remove(tmp);
                    xSum -= (long) v * curCnt;
                    if (curCnt > 1) {
                        s2.add(new RC(v, curCnt - 1));
                    }
                    while (s1.size() < x && !s2.isEmpty()) {
                        RC r = s2.last();
                        s2.remove(r);
                        s1.add(r);
                        xSum += (long) r.val * r.cnt;
                    }
                }
            }
        }
        return ans;
    }
}
```