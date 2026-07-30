[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3859\. Count Subarrays With K Distinct Integers

Hard

You are given an integer array `nums` and two integers `k` and `m`.

Return an integer denoting the count of **non-empty subarrays** of `nums` such that:

*   The subarray contains **exactly** `k` **distinct** integers.
*   Within the subarray, each **distinct** integer appears **at least** `m` times.

**Example 1:**

**Input:** nums = [1,2,1,2,2], k = 2, m = 2

**Output:** 2

**Explanation:**

The possible subarrays with `k = 2` distinct integers, each appearing at least `m = 2` times are:

| Subarray | Distinct<br>numbers | Frequency |
|---|---|---|
| `[1, 2, 1, 2]` | `{1, 2}` → `2` | `{1: 2, 2: 2}` |
| `[1, 2, 1, 2, 2]` | `{1, 2}` → `2` | `{1: 2, 2: 3}` |

Thus, the answer is 2.

**Example 2:**

**Input:** nums = [3,1,2,4], k = 2, m = 1

**Output:** 3

**Explanation:**

The possible subarrays with `k = 2` distinct integers, each appearing at least `m = 1` times are:

| Subarray | Distinct<br>numbers | Frequency |
|---|---|---|
| `[3, 1]` | `{3, 1}` → `2` | `{3: 1, 1: 1}` |
| `[1, 2]` | `{1, 2}` → `2` | `{1: 1, 2: 1}` |
| `[2, 4]` | `{2, 4}` → `2` | `{2: 1, 4: 1}` |

Thus, the answer is 3.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>
*   `1 <= k, m <= nums.length`

## Solution

```java
import java.util.HashMap;
import java.util.Map;

@SuppressWarnings("java:S6206")
public class Solution {
    public long countSubarrays(int[] nums, int k, int m) {
        int left = 0;
        int p = 0;
        long subArr = 0;
        HashMap<Integer, Integer> map = new HashMap<>();
        int valid = 0;
        for (int val : nums) {
            map.put(val, map.getOrDefault(val, 0) + 1);
            if (map.get(val) == m) {
                valid++;
            }
            WindowState state = shrinkDistinct(nums, left, p, k, m, map, valid);
            left = state.left();
            p = state.p();
            valid = state.valid();
            WindowState duplicateState = trimDuplicates(nums, left, p, m, map);
            left = duplicateState.left();
            p = duplicateState.p();
            if (map.size() == k && valid == k) {
                subArr += 1 + p;
            }
        }
        return subArr;
    }

    private WindowState shrinkDistinct(
            int[] nums, int left, int p, int k, int m, Map<Integer, Integer> map, int valid) {
        while (map.size() > k) {
            int lv = nums[left];
            if (map.get(lv) == m) {
                valid--;
            }
            map.put(lv, map.get(lv) - 1);
            if (map.get(lv) == 0) {
                map.remove(lv);
            }
            left++;
            p = 0;
        }
        return new WindowState(left, p, valid);
    }

    private WindowState trimDuplicates(
            int[] nums, int left, int p, int m, Map<Integer, Integer> map) {
        while (!map.isEmpty() && map.get(nums[left]) > m) {
            int lv = nums[left];
            map.put(lv, map.get(lv) - 1);
            left++;
            p++;
        }
        return new WindowState(left, p, 0);
    }

    private static final class WindowState {
        private final int left;
        private final int p;
        private final int valid;

        private WindowState(int left, int p, int valid) {
            this.left = left;
            this.p = p;
            this.valid = valid;
        }

        public int left() {
            return left;
        }

        public int p() {
            return p;
        }

        public int valid() {
            return valid;
        }
    }
}
```