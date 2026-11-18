[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 4007\. Widest Possible Fence

Hard

You are given an integer array `planks`, where `planks[i]` represents the height of the <code>i<sup>th</sup></code> wooden plank. Each plank has a width of 1 unit.

You want to build a fence consisting of planks that all have the **same** height.

You may either use a plank as is, or combine **exactly** two distinct original planks into a single plank whose height **equals** the sum of their heights. Each original plank can be used **at most** once, and not all original planks need to be used.

Return the **maximum possible width** of the fence that can be built.

**Example 1:**

**Input:** planks = [1,3,2,5,7,5,4,2,1]

**Output:** 4

**Explanation:**

We can have four planks of height 5.

*   `planks[3] = 5`
*   `planks[5] = 5`
*   `planks[0] + planks[6] = 1 + 4 = 5`
*   `planks[1] + planks[2] = 3 + 2 = 5`

Hence, the maximum width is 4.

**Example 2:**

**Input:** planks = [2,3,7]

**Output:** 1

**Explanation:**

*   It is impossible to form two planks of the same height, even after combining two distinct original planks.
*   Since not all original planks need to be used, we can choose any one plank as the fence.
*   Therefore, the maximum possible width is 1.

**Constraints:**

*   `1 <= planks.length <= 1000`
*   <code>1 <= planks[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int maximumWidth(int[] planks) {
        Map<Integer, Integer> freqMap = new HashMap<>();
        for (int plank : planks) {
            freqMap.merge(plank, 1, Integer::sum);
        }
        int n = freqMap.size();
        int i = 0;
        int[] nums = new int[n];
        int[] freq = new int[n];
        for (Map.Entry<Integer, Integer> entry : freqMap.entrySet()) {
            nums[i] = entry.getKey();
            freq[i++] = entry.getValue();
        }
        return getMax(n, nums, freq);
    }

    private static int getMax(int n, int[] nums, int[] freq) {
        Map<Integer, Integer> fence = HashMap.newHashMap(n * n);
        int max = 1;
        for (int right = 0; right < n; ++right) {
            int curr = nums[right];
            int count = freq[right];
            max = Math.max(max, fence.merge(curr, count, Integer::sum));
            if (count > 1) {
                max = Math.max(max, fence.merge(curr << 1, count >> 1, Integer::sum));
            }
            for (int left = right + 1; left < n; ++left) {
                max =
                        Math.max(
                                max,
                                fence.merge(
                                        curr + nums[left],
                                        Math.min(count, freq[left]),
                                        Integer::sum));
            }
        }
        return max;
    }
}
```