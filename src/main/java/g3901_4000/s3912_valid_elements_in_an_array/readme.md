[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 3912\. Valid Elements in an Array

Easy

You are given an integer array `nums`.

An element `nums[i]` is considered **valid** if it satisfies **at least** one of the following conditions:

*   It is **strictly greater** than every element to its left.
*   It is **strictly greater** than every element to its right.

The first and last elements are always valid.

Return an array of all valid elements in the same order as they appear in `nums`.

**Example 1:**

**Input:** nums = [1,2,4,2,3,2]

**Output:** [1,2,4,3,2]

**Explanation:**

*   `nums[0]` and `nums[5]` are always valid.
*   `nums[1]` and `nums[2]` are strictly greater than every element to their left.
*   `nums[4]` is strictly greater than every element to its right.
*   Thus, the answer is `[1, 2, 4, 3, 2]`.

**Example 2:**

**Input:** nums = [5,5,5,5]

**Output:** [5,5]

**Explanation:**

*   The first and last elements are always valid.
*   No other elements are strictly greater than all elements to their left or to their right.
*   Thus, the answer is `[5, 5]`.

**Example 3:**

**Input:** nums = [1]

**Output:** [1]

**Explanation:**

Since there is only one element, it is always valid. Thus, the answer is `[1]`.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`

## Solution

```java
import java.util.ArrayList;
import java.util.List;

public class Solution {
    public List<Integer> findValidElements(int[] nums) {
        List<Integer> ans = new ArrayList<>();
        int n = nums.length;
        ans.add(nums[0]);
        for (int i = 1; i < n - 1; i++) {
            boolean left = true;
            boolean right = true;
            for (int j = 0; j < i; j++) {
                if (nums[i] <= nums[j]) {
                    left = false;
                    break;
                }
            }
            for (int j = i + 1; j < n; j++) {
                if (nums[i] <= nums[j]) {
                    right = false;
                    break;
                }
            }
            if (left || right) {
                ans.add(nums[i]);
            }
        }
        if (n > 1) {
            ans.add(nums[n - 1]);
        }
        return ans;
    }
}
```