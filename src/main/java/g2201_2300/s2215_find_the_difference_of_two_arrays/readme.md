[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2215\. Find the Difference of Two Arrays

Easy

Given two **0-indexed** integer arrays `nums1` and `nums2`, return _a list_ `answer` _of size_ `2` _where:_

*   `answer[0]` _is a list of all **distinct** integers in_ `nums1` _which are **not** present in_ `nums2`_._
*   `answer[1]` _is a list of all **distinct** integers in_ `nums2` _which are **not** present in_ `nums1`.

**Note** that the integers in the lists may be returned in **any** order.

**Example 1:**

**Input:** nums1 = [1,2,3], nums2 = [2,4,6]

**Output:** [[1,3],[4,6]]

**Explanation:**

For nums1, nums1[1] = 2 is present at index 0 of nums2, whereas nums1[0] = 1 and nums1[2] = 3 are not present in nums2. Therefore, answer[0] = [1,3].

For nums2, nums2[0] = 2 is present at index 1 of nums1, whereas nums2[1] = 4 and nums2[2] = 6 are not present in nums2. Therefore, answer[1] = [4,6].

**Example 2:**

**Input:** nums1 = [1,2,3,3], nums2 = [1,1,2,2]

**Output:** [[3],[]]

**Explanation:**

For nums1, nums1[2] and nums1[3] are not present in nums2. Since nums1[2] == nums1[3], their value is only included once and answer[0] = [3].

Every integer in nums2 is present in nums1. Therefore, answer[1] = []. 

**Constraints:**

*   `1 <= nums1.length, nums2.length <= 1000`
*   `-1000 <= nums1[i], nums2[i] <= 1000`

## Solution

```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.HashSet;
import java.util.List;
import java.util.Set;

public class Solution {
    public List<List<Integer>> findDifference(int[] nums1, int[] nums2) {
        Set<Integer> set1 = createSet(nums1);
        Set<Integer> set2 = createSet(nums2);
        return Arrays.asList(getMissing(set1, set2), getMissing(set2, set1));
    }

    private Set<Integer> createSet(int[] array) {
        Set<Integer> set = new HashSet<>();
        for (int x : array) {
            set.add(x);
        }
        return set;
    }

    private List<Integer> getMissing(Set<Integer> first, Set<Integer> second) {
        List<Integer> list = new ArrayList<>();
        for (int x : first) {
            if (!second.contains(x)) {
                list.add(x);
            }
        }
        return list;
    }
}
```