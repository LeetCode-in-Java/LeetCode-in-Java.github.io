[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4011\. Count Subarrays With Even Odd Ratio I

Medium

You are given an integer array `nums` and two integers `a` and `b`.

For a non-empty subarrays, let:

*   `x` be the number of even elements.
*   `y` be the number of odd elements.

The ratio of even to odd elements in a subarray is defined as `x / y`, where ratios are compared by their exact rational values.

A subarray is considered **valid** if:

*   `y > 0`, and
*   `x / y <= a / b`.

Return the number of valid subarrays in `nums`.

**Example 1:**

**Input:** nums = [1,2,1,2], a = 3, b = 2

**Output:** 7

**Explanation:**

The following are the valid subarrays:

| Subarray | Values | Even Count | Odd Count | Ratio |
| --- | --- | --- | --- | --- |
| `nums[0..0]` | `[1]` | 0 | 1 | `0 / 1` |
| `nums[0..1]` | `[1, 2]` | 1 | 1 | `1 / 1` |
| `nums[0..2]` | `[1, 2, 1]` | 1 | 2 | `1 / 2` |
| `nums[0..3]` | `[1, 2, 1, 2]` | 2 | 2 | `2 / 2` |
| `nums[1..2]` | `[2, 1]` | 1 | 1 | `1 / 1` |
| `nums[2..2]` | `[1]` | 0 | 1 | `0 / 1` |
| `nums[2..3]` | `[1, 2]` | 1 | 1 | `1 / 1` |

Thus, the number of valid subarrays is 7.

**Example 2:**

**Input:** nums = [2,2,1], a = 2, b = 1

**Output:** 3

**Explanation:**

The following are the valid subarrays:

| Subarray | Values | Even Count | Odd Count | Ratio |
| --- | --- | --- | --- | --- |
| `nums[0..2]` | `[2, 2, 1]` | 2 | 1 | `2 / 1` |
| `nums[1..2]` | `[2, 1]` | 1 | 1 | `1 / 1` |
| `nums[2..2]` | `[1]` | 0 | 1 | `0 / 1` |

Thus, the number of valid subarrays is 3.

**Example 3:**

**Input:** nums = [2,2,2], a = 1, b = 1

**Output:** 0

**Explanation:**

Every subarray contains 0 odd numbers, so no subarray is valid.

**Constraints:**

*   `1 <= nums.length <= 1000`
*   `1 <= nums[i] <= 1000`
*   `1 <= a, b <= 1000`

## Solution

```java
public class Solution {
    private int count = 0;

    public int countRatioSubarrays(int[] nums, int a, int b) {
        count = 0;
        int n = nums.length;
        int[] p = new int[n + 1];
        for (int i = 0; i < n; i++) {
            p[i + 1] = p[i] + (nums[i] % 2 == 0 ? b : -a);
        }
        mergesort(p);
        return count;
    }

    private void mergesort(int[] p) {
        int n = p.length;
        if (n < 2) {
            return;
        }
        int[] a = new int[n / 2];
        int[] b = new int[n - n / 2];
        int idx = 0;
        for (int i = 0; i < a.length; i++) {
            a[i] = p[idx++];
        }
        for (int i = 0; i < b.length; i++) {
            b[i] = p[idx++];
        }
        mergesort(a);
        mergesort(b);
        merge(a, b, p);
    }

    private void merge(int[] a, int[] b, int[] c) {
        int i = 0;
        int j = 0;
        int k = 0;
        while (i < a.length && j < b.length) {
            if (a[i] < b[j]) {
                c[k++] = a[i++];
            } else {
                count += (a.length - i);
                c[k++] = b[j++];
            }
        }
        while (i < a.length) {
            c[k++] = a[i++];
        }
        while (j < b.length) {
            c[k++] = b[j++];
        }
    }
}
```