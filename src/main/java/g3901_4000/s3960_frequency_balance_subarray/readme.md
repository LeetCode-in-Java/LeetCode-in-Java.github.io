[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3960\. Frequency Balance Subarray

Medium

You are given an integer array `nums`.

Define a **frequency balance non-empty subarrays** as follows:

*   If the subarray contains only one distinct value, it is frequency balanced.
*   Otherwise, there must exist a positive integer `f` such that every distinct value in the subarray occurs either `f` or `2 * f` times, and both frequencies occur among the distinct values.

Return an integer denoting the length of the **longest** frequency balance subarray.

**Example 1:**

**Input:** nums = [1,2,2,1,2,3,3,3]

**Output:** 5

**Explanation:**

*   The longest frequency balance subarray is `[2, 1, 2, 3, 3]`.
*   The elements that appear most frequently are 2 and 3, both appearing twice.
*   The remaining element 1 appears once, meeting the requirements.

**Example 2:**

**Input:** nums = [5,5,5,5]

**Output:** 4

**Explanation:**

*   The longest frequency balance subarray is `[5, 5, 5, 5]`.
*   The element that appears most frequently is 5.
*   There are no other elements meeting the requirements.

**Example 3:**

**Input:** nums = [1,2,3,4]

**Output:** 1

**Explanation:**

Since all elements appear only once, the length of the longest frequency balance subarray is 1.

**Constraints:**

*   <code>1 <= nums.length <= 10<sup>3</sup></code>
*   <code>1 <= nums[i] <= 10<sup>9</sup></code>

## Solution

```java
import java.util.HashMap;
import java.util.Map;

public class Solution {
    public int getLength(int[] a) {
        int n = a.length;
        int ans = 1;
        for (int i = 0; i < n; i++) {
            Map<Integer, Integer> mp = new HashMap<>();
            int[] fq = new int[n + 1];
            int mx = 0;
            int s = 0;
            for (int j = i; j < n; j++) {
                int old = mp.getOrDefault(a[j], 0);
                if (old > 0) {
                    fq[old]--;
                }
                mp.put(a[j], old + 1);
                fq[old + 1]++;
                if (old + 1 > mx) {
                    mx = old + 1;
                    s = 1;
                } else if (old + 1 == mx) {
                    s++;
                }
                int distinct = mp.size();
                if (distinct == 1 || (mx % 2 == 0 && s < distinct && fq[mx / 2] == distinct - s)) {
                    ans = Math.max(ans, j - i + 1);
                }
            }
        }
        return ans;
    }
}
```