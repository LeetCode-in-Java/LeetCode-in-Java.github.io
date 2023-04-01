[![](https://img.shields.io/github/stars/javadev/LeetCode-in-Java?label=Stars&style=flat-square)](https://github.com/javadev/LeetCode-in-Java)
[![](https://img.shields.io/github/forks/javadev/LeetCode-in-Java?label=Fork%20me%20on%20GitHub%20&style=flat-square)](https://github.com/javadev/LeetCode-in-Java/fork)
[![](https://img.shields.io/badge/-LeetCode%20in%20Kotlin-blue?style=flat-square)](https://github.com/javadev/LeetCode-in-Kotlin)

## 2461\. Maximum Sum of Distinct Subarrays With Length K

Medium

You are given an integer array `nums` and an integer `k`. Find the maximum subarray sum of all the subarrays of `nums` that meet the following conditions:

*   The length of the subarray is `k`, and
*   All the elements of the subarray are **distinct**.

Return _the maximum subarray sum of all the subarrays that meet the conditions__._ If no subarray meets the conditions, return `0`.

_A **subarray** is a contiguous non-empty sequence of elements within an array._

**Example 1:**

**Input:** nums = [1,5,4,2,9,9,9], k = 3

**Output:** 15

**Explanation:** The subarrays of nums with length 3 are: 

- \[1,5,4] which meets the requirements and has a sum of 10. 
 
- \[5,4,2] which meets the requirements and has a sum of 11. 

- \[4,2,9] which meets the requirements and has a sum of 15. 

- \[2,9,9] which does not meet the requirements because the element 9 is repeated. 

- \[9,9,9] which does not meet the requirements because the element 9 is repeated. 

We return 15 because it is the maximum subarray sum of all the subarrays that meet the conditions

**Example 2:**

**Input:** nums = [4,4,4], k = 3

**Output:** 0

**Explanation:** The subarrays of nums with length 3 are: 

- \[4,4,4] which does not meet the requirements because the element 4 is repeated. We return 0 because no subarrays meet the conditions.

**Constraints:**

*   <code>1 <= k <= nums.length <= 10<sup>5</sup></code>
*   <code>1 <= nums[i] <= 10<sup>5</sup></code>

## Solution

```java
import java.util.HashSet;
import java.util.Set;

public class Solution {
    public long maximumSubarraySum(int[] nums, int k) {
        Set<Integer> seen = new HashSet<>();
        long sum = 0;
        long current = 0;
        int i = 0;
        int j = 0;
        while (j < nums.length) {
            while (seen.contains(nums[j])) {
                int val = nums[i++];
                seen.remove(val);
                current -= val;
            }
            seen.add(nums[j]);
            current += nums[j];
            j++;
            if (seen.size() == k) {
                sum = Math.max(sum, current);
                current -= nums[i];
                seen.remove(nums[i]);
                i++;
            }
        }
        return sum;
    }
}
```