[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)

## 2855\. Minimum Right Shifts to Sort the Array

Easy

You are given a **0-indexed** array `nums` of length `n` containing **distinct** positive integers. Return _the **minimum** number of **right shifts** required to sort_ `nums` _and_ `-1` _if this is not possible._

A **right shift** is defined as shifting the element at index `i` to index `(i + 1) % n`, for all indices.

**Example 1:**

**Input:** nums = [3,4,5,1,2]

**Output:** 2

**Explanation:** 

After the first right shift, nums = [2,3,4,5,1]. 

After the second right shift, nums = [1,2,3,4,5]. 

Now nums is sorted; therefore the answer is 2.

**Example 2:**

**Input:** nums = [1,3,5]

**Output:** 0

**Explanation:** nums is already sorted therefore, the answer is 0.

**Example 3:**

**Input:** nums = [2,1,4]

**Output:** -1

**Explanation:** It's impossible to sort the array using right shifts.

**Constraints:**

*   `1 <= nums.length <= 100`
*   `1 <= nums[i] <= 100`
*   `nums` contains distinct integers.

## Solution

```java
import java.util.List;

public class Solution {
    public int minimumRightShifts(List<Integer> nums) {
        int i;
        for (i = 1; i < nums.size(); i++) {
            if (nums.get(i) < nums.get(i - 1)) {
                break;
            }
        }
        if (nums.size() == i) {
            return 0;
        } else {
            int k;
            for (k = i + 1; k < nums.size(); k++) {
                if (nums.get(k) <= nums.get(k - 1)) {
                    break;
                }
            }
            if (k == nums.size() && nums.get(k - 1) < nums.get(0)) {
                return nums.size() - i;
            }
            return -1;
        }
    }
}
```