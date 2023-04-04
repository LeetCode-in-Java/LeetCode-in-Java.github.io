[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 349\. Intersection of Two Arrays

Easy

Given two integer arrays `nums1` and `nums2`, return _an array of their intersection_. Each element in the result must be **unique** and you may return the result in **any order**.

**Example 1:**

**Input:** nums1 = [1,2,2,1], nums2 = [2,2]

**Output:** [2]

**Example 2:**

**Input:** nums1 = [4,9,5], nums2 = [9,4,9,8,4]

**Output:** [9,4]

**Explanation:** [4,9] is also accepted.

**Constraints:**

*   `1 <= nums1.length, nums2.length <= 1000`
*   `0 <= nums1[i], nums2[i] <= 1000`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public int[] intersection(int[] nums1, int[] nums2) {
        boolean[] occ = new boolean[1001];
        for (int k : nums1) {
            occ[k] = true;
        }
        List<Integer> res = new ArrayList<>();
        for (int j : nums2) {
            if (occ[j]) {
                occ[j] = false;
                res.add(j);
            }
        }
        int[] result = new int[res.size()];
        for (int i = 0; i < res.size(); i++) {
            result[i] = res.get(i);
        }
        return result;
    }
}
```