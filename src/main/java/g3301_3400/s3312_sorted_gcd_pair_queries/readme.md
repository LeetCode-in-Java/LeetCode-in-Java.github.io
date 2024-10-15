[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3312\. Sorted GCD Pair Queries

Hard

You are given an integer array `nums` of length `n` and an integer array `queries`.

Let `gcdPairs` denote an array obtained by calculating the GCD of all possible pairs `(nums[i], nums[j])`, where `0 <= i < j < n`, and then sorting these values in **ascending** order.

For each query `queries[i]`, you need to find the element at index `queries[i]` in `gcdPairs`.

Return an integer array `answer`, where `answer[i]` is the value at `gcdPairs[queries[i]]` for each query.

The term `gcd(a, b)` denotes the **greatest common divisor** of `a` and `b`.

**Example 1:**

**Input:** nums = [2,3,4], queries = [0,2,2]

**Output:** [1,2,2]

**Explanation:**

`gcdPairs = [gcd(nums[0], nums[1]), gcd(nums[0], nums[2]), gcd(nums[1], nums[2])] = [1, 2, 1]`.

After sorting in ascending order, `gcdPairs = [1, 1, 2]`.

So, the answer is `[gcdPairs[queries[0]], gcdPairs[queries[1]], gcdPairs[queries[2]]] = [1, 2, 2]`.

**Example 2:**

**Input:** nums = [4,4,2,1], queries = [5,3,1,0]

**Output:** [4,2,1,1]

**Explanation:**

`gcdPairs` sorted in ascending order is `[1, 1, 1, 2, 2, 4]`.

**Example 3:**

**Input:** nums = [2,2], queries = [0,0]

**Output:** [2,2]

**Explanation:**

`gcdPairs = [2]`.

**Constraints:**

*   <code>2 <= n == nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 5 * 10<sup>4</sup></code>
*   <code>1 <= queries.length <= 10<sup>5</sup></code>
*   `0 <= queries[i] < n * (n - 1) / 2`

## Solution

```java
public class Solution {
    public int[] gcdValues(int[] nums, long[] queries) {
        int max = 1;
        for (int num : nums) {
            max = Math.max(max, num);
        }
        long[] gcdDp = new long[max + 1];
        for (int num : nums) {
            gcdDp[num]++;
        }
        for (int i = 1; i <= max; i++) {
            long count = 0;
            for (int j = i; j <= max; j = j + i) {
                count += gcdDp[j];
            }
            gcdDp[i] = ((count - 1) * count) / 2;
        }
        for (int i = max; i > 0; i--) {
            for (int j = i + i; j <= max; j = j + i) {
                gcdDp[i] -= gcdDp[j];
            }
        }
        for (int i = 1; i <= max; i++) {
            gcdDp[i] += gcdDp[i - 1];
        }
        int[] result = new int[queries.length];
        for (int i = 0; i < queries.length; i++) {
            result[i] = binarySearch(max, gcdDp, queries[i] + 1);
        }
        return result;
    }

    private int binarySearch(int n, long[] arr, long val) {
        int l = 1;
        int r = n;
        while (l < r) {
            int mid = l + (r - l) / 2;
            if (arr[mid] < val) {
                l = mid + 1;
            } else {
                r = mid;
            }
        }
        return l;
    }
}
```